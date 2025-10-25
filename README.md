# 🧭 Navigation Menu Builder (Gainsight CC)

A zero-dependency, in-browser tool to import your **Gainsight Customer Community** header HTML, restructure it into a clean, nested menu, preview hover animations, and export ready-to-paste snippets.

## Why this exists

Gainsight’s header HTML flattens “section” categories (e.g. **Concur Expense**, **Concur Invoice**, **Concur Travel**, **Resources**). This tool imports that markup and **groups items by section headers** (anchors containing `<strong>`), turning them into parents with their following siblings as children—matching the logical community structure you actually want.

## What’s included

- **Gainsight-only importer**  
  Parses `.header-navigation-items_menu` and the product-forums mega menu.  
  Section headers (links with `<strong>`) → parents; subsequent items → children.  
  Preserves true nested flyouts (e.g., *Welcome → Getting started*, *Admins Section → Admin Resources / Admin Onboarding / What’s new*).

- **Drag-and-drop editor**  
  Reorder, nest/unnest, add, and delete items. Per-item: label, URL, target, colors, animation, roles. Root items get an optional `position`.

- **Live preview**  
  Multi-level dropdown preview with underline / rise / pulse hover animations.

- **Exports**  
  - **HTML/CSS**: standalone navbar snippet.  
  - **Gainsight CC**: `window.customMegaMenuItems` head snippet + optional animation CSS.  
  - **JSON**: raw schema.

- **Local persistence + Undo/Redo**  
  Stores in `localStorage`; full undo/redo history.

## Getting started

1. Open `index.html` (or `cc-navigation-menu-builder.html`) in any modern browser.
2. In **Import from Gainsight HTML**, paste your site’s header HTML (copy from your live page or theme).
3. (Optional) Check **Replace current items** if you want to wipe existing items.
4. Click **Import**.  
   You should see:
   - Quick links (Community / Recently active / Unanswered…)  
   - Grouped sections (e.g., **Concur Expense** with *Show & Tell*, *Discussions*, *Welcome* under it)  
   - Preserved flyouts (e.g., *Welcome → Getting started*, *Admins Section → …*)

> Tip: If your header uses a mega menu entry (e.g., *Product Forums*), we read its `.dropdown--forums-overview` internals and group sections automatically.

## Editing

- Click **＋ Add root item** to create a new top-level entry.
- Use the **≡ handle** to drag and drop. Drop inside another item to nest.
- Click the caret button to **collapse/expand**. **Alt+Click** toggles the whole branch.
- Use the pill controls to set text/background color, animation, target, roles, etc.

## Exporting

- **Export HTML/CSS** → `navigation.html` (copy into any site).
- **Export Gainsight CC** → `customMegaMenuItems_head_snippet.html`  
  Paste the `<script>` + `<style>` into your community **HEAD**.  
  Add a class such as `anim-underline` to your header container to enable hover animations.
- **Export JSON** for backups or automation.

## Keyboard shortcuts

- **Undo**: `Cmd/Ctrl+Z`  
- **Redo**: `Cmd/Ctrl+Shift+Z` or `Cmd/Ctrl+Y`

## How the importer works (Gainsight-only)

1. Locate the UL at `.header-navigation-items_menu`.
2. For a mega-menu item (e.g., *Product Forums*), inspect its dropdown container:
   - Read **quick links** list.
   - Walk the **main list**:
     - If an anchor contains `<strong>`, it is a **section header** → becomes a **parent**.
     - Subsequent siblings until the next header become that parent’s **children**.
     - If a list item contains a nested flyout (`.dropdown--forums-overview--nested`), recurse and attach those grand-children.
3. Build a clean tree and render.

> We intentionally **removed generic UL fallbacks and aggressive de-duplication**. The importer is focused on Gainsight markup to keep behavior predictable and clean.

## Data shape

```ts
type MenuItem = {
  id: string;
  label: string;
  href: string;       // "#" for containers
  target: "_self" | "_blank";
  color: string;      // e.g., "#e5e7eb"
  bg: string;         // "" or hex
  anim: "inherit" | "underline" | "rise" | "pulse" | "none";
  roles: string[];    // optional role ids
  position?: number;  // only used for top-level items on export
  children: MenuItem[];
};
```

## Troubleshooting

- **“Could not find Gainsight header UL”**  
  Ensure your paste includes a `<nav>` with `.header-navigation-items_menu`. Copy directly from your live header.
- **Item appears flat instead of nested**  
  Verify that the intended parent anchor has a `<strong>` inside; that’s how section grouping is detected.
- **Preview shows no hover animation**  
  Set the global animation dropdown or per-item `Anim` to something other than `inherit`.

## Development

- No build step required. Open the HTML file in a browser.
- Uses SortableJS (CDN) for drag/drop.
- All state is local to the browser via `localStorage`.

## License

MIT

---

## Conventional commit + changelog snippet

**Commit (single line):**
```
feat(import): Gainsight-only importer with section-header grouping; remove generic fallback & duplication; preserve nested flyouts; update docs
```

**Extended message:**
```
feat(import): Gainsight-only importer with section-header grouping

- Parse .header-navigation-items_menu and forums mega menu only
- Treat anchors containing <strong> as section headers (parents); subsequent items as children
- Preserve true nested flyouts (e.g., Welcome → Getting started; Admins Section → Admin Resources / Admin Onboarding / What’s new)
- Remove generic UL fallback and heavy de-duplication logic
- Keep local undo/redo and preview intact
- Update README with usage, importer details, exports, and troubleshooting
```
