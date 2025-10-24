# 🧭 Gainsight Customer Community – Navigation Menu Builder

A zero-install, single-file HTML app to **import, build, rearrange, preview, and export** navigation menus for **Gainsight Customer Community (CC)** and general websites.

## Features

- **Drag & drop** with unlimited nesting (SortableJS)
- Per-item properties: label, href, target, text color, background, animation, roles (**IDs**), root position
- **Undo / Redo**
- **Live Preview** with multi-level dropdowns and caret indicators
- **Resizable layout** (drag the vertical divider)
- **Collapse / expand (“twirl”)** editor nodes (Alt/Option-click to toggle an entire subtree)
- **Import from HTML** (simple UI):
  - Source: **HTML File** or **Paste HTML**
  - **Promote wrapper to root** (skip a single outer wrapper)
  - **Replace current items**
  - **Auto-nest by URL path** (helps when the source is flat)
- **Exports**
  - **Gainsight CC** (primary): one file containing `<script>` + `<style>` (hover animation utilities)
  - **HTML/CSS** (via “⋯ More” menu): `<nav>` + fully bundled CSS (dropdowns + animations)
  - **JSON** (via “⋯ More” menu)

---

## Quick Start

1. Open `navigation_menu_builder.html` in a browser (no server needed).
2. Use **+ Add root item** to start, or open **Import from HTML** to bootstrap from existing markup.
3. Drag the **center divider** to resize the editor vs. preview.
4. Hover items with a ▾ caret in the **Live Preview** to see dropdowns/flyouts.
5. Use **Undo/Redo** as needed.

---

## Importing from HTML

Open **Import from HTML** (collapsed by default):

- **Source**: choose **HTML File** or **Paste HTML**, then click **Import**.
- **Advanced** (optional):
  - **Promote wrapper to root** – If the source HTML has one outer item (“Products”, “Community”), import its children as top-level items.
  - **Replace current items** – Overwrite instead of appending.
  - **Auto-nest by URL path** – Generates submenus based on URL path segments when the source is flat.

> The importer strips common trailing counters (e.g., `“Zoom AI Companion 0” → “Zoom AI Companion”`).

---

## Editing Items

- Click the small ▾ button to **collapse/expand** a node.
- **Roles** should be **IDs** (e.g., `10, 11, 12`), not names like “admin, moderator”.
- Top-level items can have a **Position** (used by CC’s `position` field).

---

## Exports

### 1) Gainsight Customer Community (Primary)

Click **Export Gainsight CC**. A file like:
customMegaMenuItems_head_snippet.html

will download containing **both**:

- A `<script>` that defines `window.customMegaMenuItems = [...]`
- A `<style>` block with **optional hover animation utilities** (underline / rise / pulse)

**Where to paste:**  
**Gainsight Customer Community → Admin → Third-party scripts → `<HEAD>`**

**How to enable animations:**  
Add **one** of the following classes to a wrapper in your theme (e.g., on `<body>` or the header container):

- `anim-underline`
- `anim-rise`
- `anim-pulse`

> The utility CSS targets common CC link classes: `.main-menu-link` and `.header-navigation_link`.  
> If your theme uses different classes, adjust the selectors in the exported `<style>`.

**Notes:**
- Only **top-level items that have children** are exported (prevents empty carets/blank dropdowns in CC).
- If an item has children and no URL (or `#`), it’s treated as a **container** in the CC children mapping.
- `roles` in the builder map directly to `roles: [...]` in the CC export (enter IDs like `10, 11, 12`).

### 2) HTML/CSS (More → Export HTML/CSS)

- Exports a standalone `<nav>` and an inline `<style>` with:
  - Base navbar
  - Multi-level dropdowns (flyouts)
  - Caret styling
  - Hover animations (`underline`, `rise`, `pulse`)
- The global animation class is applied to the `<nav>`; per-item overrides are preserved.

### 3) JSON (More → Export JSON)

- Exports the internal data structure for your own tooling.

---

## Keyboard Shortcuts

- **Undo**: `Cmd/Ctrl + Z`  
- **Redo**: `Cmd/Ctrl + Shift + Z` (or `Cmd/Ctrl + Y`)

---

## Tips

- Use **twirl** to collapse busy branches while rearranging.
- If your import comes in “flat,” try **Auto-nest by URL path**.
- If you don’t want the outer “wrapper” (e.g., “Products”), enable **Promote wrapper to root**.

---

## Known Limitations

- HTML import is best-effort across varied markups; if something doesn’t parse, share a snippet and the selectors can be extended.
- The CC export sets up data (via `window.customMegaMenuItems`); layout/behavior is controlled by your CC theme. The included animation CSS is optional and additive.

---

## License

MIT
