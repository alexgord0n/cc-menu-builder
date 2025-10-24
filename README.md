# Navigation Menu Builder

A browser-based, drag-and-drop builder for creating complex navigation menus.  
Works entirely client-side (no backend) and includes multi-level nesting, live preview, deep HTML import, JSON/HTML/Gainsight CC export, undo/redo, collapsible nodes, and a resizable editor.

---

## Features

- Drag & drop editor (reorder + nest with a handle)
- Live preview with multi-level hover dropdowns and carets
- Per-item controls: label, href, target, text color, background color, animation, roles
- Root control: position (used by Gainsight CC export)
- Collapsible nodes (twirl ▾/▸). Alt/Option-click to collapse/expand a whole subtree
- Undo/Redo (`Ctrl/Cmd+Z`, `Ctrl/Cmd+Shift+Z`)
- Autosave in `localStorage`
- Resizable layout (drag the vertical divider; double-click to reset)
- Import: JSON (flat or nested), Deep HTML (mega menus, dropdown containers)
- Export: JSON, HTML+CSS (standalone), Gainsight Customer Community (CC **HEAD snippet with `<script>` tags**)

---

## Getting Started

1. **Download or clone** the repo and open the builder in a browser.
   ```bash
   git clone https://github.com/<your-org>/<your-repo>.git
   cd <your-repo>
   # Option A: double-click the HTML file (e.g., nav_builder.html)
   # Option B: serve locally
   python3 -m http.server 8080
   # then visit http://localhost:8080/nav_builder.html
   ```

2. **Build a menu**
   - Click **+ Add root item** to add a top-level entry.
   - Use the **≡ handle** to drag for reordering or nesting.
   - Click **＋** on an item to add a child.
   - Click the **twirl** ▾ to collapse/expand; **Alt/Option+click** twirls the entire branch.

3. **Preview**
   - Hover items with a caret to reveal children.
   - Set a **Global animation** (Underline, Rise, Pulse, None) or override per-item.

---

## Importing

### Import JSON
- Accepts **nested arrays** (`children`) or **flat arrays** (with `parentId`).
- Options:
  - **Promote to root** — lift imported items to top level (no wrapper).
  - **Replace current** — overwrite the current tree.

### Import HTML (Deep)
Paste or upload existing site nav markup. The importer detects:
- Standard `<ul><li><a>` structures
- Mega-menu patterns (e.g., `.dropdown-container`, `.main-menu-list`, etc.)
- Labels from `.main-menu-link__name` / `<strong>` (ignores counters like `.text--meta`)
- Optional: **Auto-nest by URL path** if the markup is flat

> If your site uses unique wrappers/classes, extend the selector list in the importer as needed.

---

## Exporting

### JSON
Clean structural representation of your menu (IDs, labels, hrefs, children, etc.).

### HTML + CSS
Standalone snippet for a multi-level dropdown nav (carets, hover interactions, animations).  
Copy directly into static sites or prototypes.

### Gainsight Customer Community (CC)
Exports a **HEAD snippet** (HTML file that wraps your data in `<script>` tags) so you can paste it directly into Gainsight Community Admin:

1. In the builder, click **Export Gainsight CC** → this downloads `customMegaMenuItems_head_snippet.html`.
2. Open the file and copy its contents. It looks like:
   ```html
   <!-- Gainsight Customer Community custom menu -->
   <script>
   window.customMegaMenuItems = [ /* …your data… */ ];
   </script>
   ```
3. In **Gainsight Customer Community Admin** go to **Third-party scripts** → **HEAD** and paste the snippet there.
4. Save/Publish.

**Notes**
- Each **root with children** becomes a CC dropdown (`key: "custom-dropdown"`).  
- **Roots without children are omitted** (prevents empty carets/blank dropdowns).  
- `position` is taken from the root item (or auto-numbered).  
- `roles` comes from the item’s Roles field (array).  
- Children with **children and no href** export with `isContainer: true`.

---

## Shortcuts

- Undo: `Ctrl/Cmd + Z`  
- Redo: `Ctrl/Cmd + Shift + Z`  
- Collapse/expand subtree: **Alt/Option + click** the twirl ▾/▸

---

## UI Tips

- **Transparent backgrounds**: the editor stores empty `bg` as transparent; the preview respects it (no invalid `transparent` in `<input type="color">`).
- **Resizable editor**: drag the vertical divider between the editor and preview. Double-click to reset. Size persists in `localStorage`.

---

## Troubleshooting

- **Labels show a trailing “0”**  
  Your source HTML likely includes counters like `<span class="text--meta">0</span>`.  
  The importer strips those by reading the label from `.main-menu-link__name` (then removing trailing counts).

- **Divider looks off / won’t resize correctly**  
  Ensure there is **one** wrapper:
  ```html
  <div class="layout">
    <section id="editor">…</section>
    <div id="divider"></div>
    <section>…preview…</section>
  </div>
  ```
  The divider must be a **direct child** between the two panels.

- **Nothing imports from HTML**  
  Confirm your markup contains a `<nav>` or `<ul>` with `<li><a>` links.  
  If it’s highly custom, add/adjust selectors in the importer.

---

## Roadmap

- Token-based import from Gainsight CC APIs (via local proxy to avoid CORS)
- One-click **“Groups”** dropdown builder from API data
- **Collapse/Expand All** toolbar actions
- Theming presets for HTML/CSS export

---

## Contributing

PRs welcome! For site-specific import rules, keep selectors narrow and wrap behind options.

---

## License

MIT
