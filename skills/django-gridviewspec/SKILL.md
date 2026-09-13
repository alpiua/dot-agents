---
name: django-gridviewspec
description: Architecture standards, lifecycle invariants, Islands Architecture (GridViewSpec + HTMX + Alpine), and MCP tooling for Django UI development.
---

# Django + GridViewSpec Development Guide

This skill governs the development, integration, and maintenance of user interfaces built with **Django**, **GridViewSpec**, **HTMX**, and **Alpine.js**.

---

## 1. Core Architecture: Islands Architecture

Modern ContextUnity/ContextCommerce applications avoid both heavy client-side SPAs and overly rigid purely-declarative UIs by adopting an **Islands Architecture**:

```text
┌─────────────────────────────────────────────────────────────┐
│                    Django Host Layout                       │
│                                                             │
│  ┌───────────────────────────────────────────────────────┐  │
│  │     HTMX Viewport (hx-boost, partial DOM swaps)       │  │
│  │                                                       │  │
│  │   ┌───────────────────────────────────────────────┐   │  │
│  │   │  GridViewSpec Backbone                        │   │  │
│  │   │  - Page layout areas (stack, table-card, etc) │   │  │
│  │   │  - AG Grid / SimpleTable instances            │   │  │
│  │   │  - Toolbar filters & KPI metric cards         │   │  │
│  │   │  - Smart search & server-side sorting/export  │   │  │
│  │   └───────────────────────────────────────────────┘   │  │
│  │                                                       │  │
│  │   ┌───────────────────────────────────────────────┐   │  │
│  │   │  Isolated Alpine.js Islands (Micro-UX only)   │   │  │
│  │   │  - Image lightbox gallery (zoom, next/prev)   │   │  │
│  │   │  - In-memory form draft state (x-model)       │   │  │
│  │   │  - Quick client popovers & inline toggles     │   │  │
│  │   └───────────────────────────────────────────────┘   │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Strict Boundaries for Alpine.js
1. **Rule 1 (Zero Global State):**
   - **Never** attach Alpine component references to `window` (e.g. `window._productsRef = this` or `window._stockRef = this`).
   - Alpine components must be self-contained within their host DOM element (`x-data`). Communication across blocks occurs strictly via standard DOM events (`window.dispatchEvent(new CustomEvent(...))`) or `GridView.registerAction`.
2. **Rule 2 (No Page Navigation / Tab Routing):**
   - Alpine must never manage page routes, browser history, or top-level tabs.
   - Tabs and screen transitions are managed by **HTMX** (`hx-boost="true"`, `hx-push-url="true"`) or **GridViewTabs** (`initTabGroups`).
3. **Rule 3 (Stateless Across HTMX Swaps):**
   - Alpine components must be resilient to DOM replacement. When HTMX swaps a container, `Alpine.initTree(scope)` reinitializes fresh DOM nodes without expecting preserved in-memory references.

---

## 2. Django + GridViewSpec Integration Patterns

### 2.1 Backend Page Pattern
Every grid-enabled screen must follow the two-tier structure:
1. **`page_data.py` (Data & Spec Builder):**
   - Precomputes counts and stats in SQL (do not fetch tens of thousands of rows into Python).
   - Returns a `GridViewSpec` object containing table definitions, toolbar filters, KPI blocks, and column configurations.
2. **`views.py` (Thin Controller):**
   - Calls `page_data(request)`.
   - Passes `page` to template (or JSON for infinite scroll/options API).
   - Never builds manual `columnDefs` in templates.

### 2.2 Template Asset & Script Order
In `_base.html`, assets must load in this exact order:
```django
<head>
    <!-- 1. GridView CSS -->
    {% grid_view_spec_assets part='css' %}
    <!-- 2. GridView JS in head so window.GridView is available for page scripts -->
    {% grid_view_spec_assets part='js' force_core=True %}
    <!-- 3. HTMX & Alpine -->
    <script src="{% static 'pim/js/vendor/htmx.min.js' %}"></script>
    <script defer src="{% static 'pim/js/vendor/alpine.min.js' %}"></script>
</head>
<body>
    <div id="cm-pim-main-area">
        {% block body %}{% endblock %}
        <div id="cm-pim-page-scripts" class="hidden">
            {% block scripts %}{% endblock %}
            {% block after_grid_view_js %}{% endblock %}
        </div>
    </div>
</body>
```

### 2.3 Resilient Renderer & Action Registration
Never assume `window.GridView` or `registerRenderer` is ready synchronously in a single execution tick. Wrap hook registrations in self-retrying closures:
```javascript
(function registerHooks() {
    var gv = window.GridView;
    if (!gv || typeof gv.registerRenderer !== 'function') {
        setTimeout(registerHooks, 30);
        return;
    }
    if (gv.__myHooksRegistered) return;
    gv.__myHooksRegistered = true;

    gv.registerRenderer('custom_badge', function (params) {
        var val = params && params.value;
        return '<span class="cm-badge">' + (val || '') + '</span>';
    });

    gv.registerAction('open_detail', function (ctx) {
        window.dispatchEvent(new CustomEvent('cm:open-detail', { detail: { id: ctx.rowId } }));
    });
})();
```

---

## 3. HTMX Lifecycle & Grid Clean-Up Invariants

### 3.1 `htmx:beforeSwap` — Safe Grid Destruction
`window.GridView.byId` is an object with methods `{ register, get, registerBoot, boot }` — **it is NOT a Map or an Array**. Never call `.forEach()` or `.clear()` on it.
```javascript
document.body.addEventListener('htmx:beforeSwap', function (e) {
    try {
        document.querySelectorAll('.cm-grid-shell[data-grid-id], [data-cm-table-grid-id]').forEach(function (el) {
            var gridId = el.getAttribute('data-grid-id') || el.getAttribute('data-cm-table-grid-id');
            if (gridId && window.GridView && window.GridView.byId && typeof window.GridView.byId.get === 'function') {
                var handle = window.GridView.byId.get(gridId);
                if (handle && handle.gridApi && typeof handle.gridApi.destroy === 'function') {
                    try { handle.gridApi.destroy(); } catch (_) {}
                    handle.gridApi = null;
                }
            }
        });
    } catch (err) {
        console.warn('[HTMX beforeSwap] Grid cleanup error:', err);
    }
});
```

### 3.2 `htmx:afterSwap` — Live DOM Target Resolution
When using `hx-swap="outerHTML"`, `e.detail.target` is the **disconnected, dead DOM node** (`isConnected === false`). Always resolve to the connected DOM node:
```javascript
document.body.addEventListener('htmx:afterSwap', function (e) {
    var target = e.detail && e.detail.target;
    var mainArea = document.getElementById('cm-pim-main-area');
    // Guard against disconnected target:
    var scope = (target && target.isConnected) ? target : (mainArea || document);

    if (window.Alpine && scope) {
        try { window.Alpine.initTree(scope); } catch (err) { console.warn(err); }
    }
    if (window.GridView && scope) {
        try {
            if (typeof window.GridView.boot === 'function') window.GridView.boot(scope);
            else if (typeof window.GridView.bootScope === 'function') window.GridView.bootScope(scope);
        } catch (err) { console.warn(err); }
    }
    if (window.lucide && typeof window.lucide.createIcons === 'function') {
        window.lucide.createIcons();
    }
});
```

---

## 4. Overlay ownership and stacking

**Package (`grid-view-spec`)** owns toolbar search, filter panels, and the column-settings dialog. Use tokens in `frontend/styles/tokens.css`: `--cm-z-search` (10), `--cm-z-filter-panel` (250), `--cm-z-search-active` (260), `--cm-z-settings` (1300). Column dialog width is `--cm-table-settings-dialog-width`. Change those only in the package, then `npm run build` in `frontend/`.

**Host** owns inspector drawer, product edit modal, gallery lightbox, and cart popovers. Put their `z-index` between `--cm-z-filter-panel` and `--cm-z-settings` (PIM uses 490–600).

Mount Alpine islands on a dedicated root (for example `.cm-pim-islands`), not the page `<main>`. Tabs and `?tab=` stay HTMX / `GridViewTabs`. Islands talk through `CustomEvent` and `GridView.registerAction`.

Escape, top overlay first: gallery, then modal, then drawer. Backdrop `@click` closes; `@click.stop` only on the inner card and `‹` `›`.

---

## 5. MCP Tools & Skills Ecosystem

When working on Django + GridViewSpec, leverage these specialized tools:

### `gridviewspec` MCP Tools
- **`gridview_catalog`**: Inspect available block types, registries (renderers, actions, validators), and visual themes.
- **`gridview_schema`**: Retrieve JSON schema for `GridViewSpec`, `GridViewBlock`, `GridViewTable`, etc.
- **`gridview_validate`**: Validate any Python dictionary or JSON spec against the official schema (`ok: true`).
- **`gridview_examples`**: Look up canonical reference implementations for tabs (`tabs_area_refs`), overlays, and complex tables.
- **`gridview_normalize`**: Clean and normalize hand-authored specs to standard wire format.

### `contextunity-forge` MCP Tools
- **`code_map_inspect`**: Inspect backend view functions, contract types, and data dependencies.
- **`ast_grep_search`**: Find exact structural patterns across Python and HTML templates.

### `playwright` MCP Tools
- **`browser_navigate`**: Load the live page (default port 8080).
- **`browser_evaluate`**: Inspect live JavaScript objects (`window.GridView.byId.get(...)`, `gridApi.getDisplayedRowCount()`).
- **`browser_click`**: Verify tab swaps, modal triggers, and drawer expansions.
- **`browser_take_screenshot`**: Verify visual layout, responsive margins, and theme styling.

### Related Skills
- **`commerce-development`**: Host bindings, Traverse setup, and package specifics.
- **`type-validation`**: Strict BasedPyright and Ruff checks for Django views and models.
---

## 6. Internationalization & Localization (i18n): English Base Invariant

- **English Base Strings**:
  - All Python specs, table column headers, filter titles, badge labels, action descriptions, and model fields MUST be defined in English.
  - Template markup must wrap all user-facing copy in `{% translate "..." %}` with English source strings.
  - JavaScript UI helpers (placeholders, tooltips, alert messages) must default to English base text.
  - **STRICT INVARIANT**: Hardcoding Ukrainian or other non-English languages directly into Python, HTML templates, or JS files is strictly forbidden.
- **Translation Pipeline**:
  - Non-English languages are maintained exclusively via `django.po` and compiled to `django.mo`.
  - When new strings are created: `makemessages -l uk` → edit `django.po` → `compilemessages -l uk`.

---

## 7. Verification Protocol

Before declaring any change complete:
1. **Run Pytest Suite:**
   ```bash
   uv run pytest extensions/commerce/tests/ -q
   ```
2. **Validate Spec:**
   Run `gridview_validate` on newly introduced or modified specs.
3. **Live Browser Verification (Playwright):**
   - Verify tab navigation updates URL without full page reload or white flash.
   - Verify AG Grid table renders data rows (`gridApi.getDisplayedRowCount() > 0`).
   - Verify overlays open and close via Escape and click-outside without side-effects.
