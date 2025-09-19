🧭 Navigation Menu Builder

A browser-based, drag-and-drop builder for creating complex navigation menus.
Supports multi-level nesting, styling/animations, deep HTML import, JSON/HTML/Gainsight CC export, a live preview, undo/redo, and a resizable editor — all client-side, no backend.

⸻

✨ Features
	•	Drag & drop editor (reorder + nest with a handle)
	•	Live preview with multi-level hover dropdowns and carets
	•	Per-item controls: label, href, target, text color, background color, animation, roles
	•	Root controls: position (used by Gainsight CC export)
	•	Collapsible nodes (twirl ▾/▸), plus Alt/Option-click to collapse/expand a whole subtree
	•	Undo/Redo (Ctrl/Cmd+Z, Ctrl/Cmd+Shift+Z)
	•	Autosave in localStorage
	•	Resizable layout (drag the vertical divider; double-click to reset)
	•	Import sources
	•	JSON (flat or nested)
	•	Deep HTML (mega menus, dropdown containers, etc.)
	•	Export targets
	•	JSON
	•	HTML + CSS (standalone snippet)
	•	Gainsight Customer Community (window.customMegaMenuItems)

⸻

🚀 Getting Started
	1.	Clone / Download the repo and open the builder:

git clone https://github.com/<your-org>/<your-repo>.git
cd <your-repo>
# Open in your browser (any static server or just double-click the HTML file)
# macOS:
open nav_builder.html
# or serve:
python3 -m http.server 8080
# then visit http://localhost:8080/nav_builder.html


	2.	Build a menu
	•	Click + Add root item to add a top-level entry.
	•	Use the ≡ handle to drag for reordering or nesting.
	•	＋ adds a child to the current item.
	•	Click the twirl ▾ to collapse/expand; Alt/Option+click twirls the whole branch.
	3.	Preview & polish
	•	Hover items with a caret to reveal children.
	•	Choose a Global animation (Underline, Rise, Pulse, None) or override per-item.

⸻

📥 Importing

Import JSON
	•	Accepts nested arrays (children) or flat arrays (with parentId).
	•	Options:
	•	Promote to root — lift imported items to top level (no wrapper).
	•	Replace current — overwrite the current tree.

Import HTML (Deep)

Paste or upload existing site nav markup. The importer detects:
	•	<ul><li><a> structures
	•	Mega-menu patterns (e.g., .dropdown-container, .main-menu-list, etc.)
	•	Label extraction from .main-menu-link__name (ignores counters like .text--meta)
	•	Optional: Auto-nest by URL path if the markup is flat.

Tip: If a specific site uses unique wrappers/classes, you can extend the selector list in the importer.

⸻

📤 Exporting

JSON

A clean structural representation of your menu.

HTML + CSS

A standalone, multi-level dropdown nav (carets, hover interactions, animations).
Good for quickly embedding into static sites or prototypes.

Gainsight Customer Community (CC)

Exports window.customMegaMenuItems objects:
	•	Each root with children becomes a CC dropdown item:
	•	key: "custom-dropdown" (required by CC)
	•	position from your root item (or auto-numbered)
	•	roles from the item’s Roles field (array)
	•	Roots without children are omitted (prevents empty carets/blank dropdowns).
	•	Children map to CC items; a child with children and no href is exported as isContainer: true.

⸻

⌨️ Shortcuts
	•	Undo: Ctrl/Cmd + Z
	•	Redo: Ctrl/Cmd + Shift + Z
	•	Collapse/expand subtree: Alt/Option + click the twirl ▾/▸

⸻

🧩 UI Tips
	•	Transparent backgrounds: the editor stores empty bg as “transparent”; the preview respects it, and the color input stays valid (no transparent in <input type="color">).
	•	Resizable editor: Drag the vertical divider between the editor and preview. Double-click the divider to reset. Size persists in localStorage.

⸻

🧪 Troubleshooting
	•	Labels show a trailing “0”
Your source HTML likely includes counters like <span class="text--meta">0</span>.
The importer strips those on import via .main-menu-link__name detection.
	•	Divider looks off / doesn’t resize correctly
Ensure there is only one <div class="layout"> wrapper containing exactly three direct children:
	1.	left panel, 2) <div id="divider">, 3) right panel.
	•	Nothing imports from HTML
Verify your markup contains a <nav> or a <ul> with <li><a> links.
If it’s highly custom, share a small snippet and extend the selectors in the importer.

⸻

🗺 Roadmap
	•	Import from Gainsight CC API (token-based), with a local proxy to avoid CORS.
	•	One-click “Groups” dropdown builder from API data.
	•	Collapse/Expand All toolbar actions.
	•	Custom theming presets for the HTML/CSS export.

⸻

🤝 Contributing

PRs welcome! If you’re adding site-specific import rules, keep selectors narrow and guarded behind feature flags or options.

⸻

📄 License

MIT — free to use, modify, and adapt.

⸻

📸 Screenshots (optional)

Add images/GIFs of the editor, import, and exports here.
