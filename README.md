# ECO.SYSTEM.OS

**A browser-based, force-directed network diagram and relationship mapping tool.**

No build step. No server. No installation. Open in any browser.

---

## What It Is

ECO.SYSTEM.OS is a single-file, zero-dependency web application for creating, visualising, and navigating relationship networks — nodes connected by directed links, simulated with D3.js physics. It runs entirely in the browser with no backend required.

Use it to map teams, systems, processes, recipes, concepts, dependencies, knowledge graphs, organisations, or any network of interconnected entities.

---

## Quickstart

1. Download `index.html`
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge)
3. Click **+ Node** to start building — or **Import** a saved `.json` file

That's it.

---

## Features

### Network Editing
- **Add nodes** with name, type, status, importance (1–100), color, tags, notes, and custom key-value properties
- **Create directed links** between nodes with optional relationship labels
- **Drag** nodes freely on the canvas; **pin** them in place
- **Double-click** or **right-click** a node to open the editor
- **Delete** nodes with single-step undo (`Ctrl+Z`)

### Visualisation
- **Force-directed layout** — nodes simulate physical repulsion and link attraction via D3.js
- **3D Force Graph** — full 3D WebGL view
- **Importance Rings** — concentric ring layout grouping nodes by importance tier
- **Animated link particles** — directional flow along every link (toggle pause/play with the Links button)
- **Node shapes** — filled circles, outline circles, or cards (rectangles)
- **Color by Type** — assigns distinct colours per node type
- **Canvas background** — custom colour picker

### Properties Panel
- Click any node to inspect **type, status, importance, tags, notes, custom properties,** and connections
- Navigate between **Properties / Index / Tags / Stats** tabs
- **Index** tab ranks all nodes by importance and connection count
- **Stats** tab shows graph metrics, pie charts, and most-connected nodes

### Search & Filter
- **Inline search bar** — dims non-matching nodes in real time
- **Ctrl+K Quick Switcher** — keyboard-navigable node search overlay
- **Filter bar** — filter by type, status, tag, or orphan nodes

### Save & Load
- **Save as JSON** — full portable graph export
- **Save as SVG** — vector export of the 2D canvas
- **Browser storage** — auto-saves to `localStorage`; manual save/load supported
- **Drag & drop** — drop any `.json` file onto the canvas to import

### Keyboard Shortcuts

| Key | Action |
|---|---|
| `Ctrl+K` | Quick node switcher |
| `F` | Focus mode on hovered node |
| `Esc` | Close modal / exit focus mode |
| `Ctrl+Z` | Undo last deletion |
| `Dbl-click` | Edit node |
| `Right-click` | Edit node |
| `?` | Keyboard shortcuts help |

---

## Technology

| Library | Version | Purpose |
|---|---|---|
| [D3.js](https://d3js.org) | 7.8.5 | Force simulation, SVG rendering, zoom |
| [3d-force-graph](https://github.com/vasturiano/3d-force-graph) | 1.73.4 | 3D WebGL graph |
| [GSAP](https://gsap.com) | 3.12.2 | UI animations |
| [Sunflower](https://fonts.google.com/specimen/Sunflower) | — | Typography (Google Fonts) |

All libraries loaded from CDN. No npm, no bundler, no local dependencies.

---

## File Structure

```
index.html   ← the entire application (HTML + CSS + JS, single file)
README.md    ← this file
```

---

## Browser Support

All modern evergreen browsers. WebGL required for 3D mode.

---

## Data Format

JSON files follow this structure. The `_schema` key is optional — it is stripped on import and can be used to embed documentation or universal diagram rules alongside your data.

```json
{
  "_schema": { },
  "version": "1.041",
  "nodes": [
    {
      "id": "n1a2b3c4",
      "name": "Node Name",
      "type": "System",
      "status": "Active",
      "importance": 75,
      "color": "#2203ff",
      "owners": ["Alice"],
      "tags": ["core"],
      "notes": "One-sentence qualitative note.",
      "linksOut": ["n5d6e7f8"],
      "linksIn": [],
      "custom": [{ "key": "Version", "val": "2.1" }],
      "linkMeta": { "n5d6e7f8": { "label": "depends on" } },
      "pinned": false,
      "updatedAt": "2026-03-05T10:00:00.000Z"
    }
  ],
  "links": [],
  "customTypes": ["Dish", "Process", "Ingredient"],
  "customStatuses": []
}
```

**Key rules:**
- `nodes` is the source of truth. `links` is always rebuilt from `linksOut` on import — do not hand-craft it.
- `importance` maps to visual node size: `1` → 8px radius, `100` → 58px. Spread the full range intentionally.
- `linksOut` holds IDs of nodes this node governs or flows into. `linksIn` is derived automatically.
- `customTypes` and `customStatuses` are merged with the app defaults on import.

### Universal Diagram Structure

ECO.SYSTEM.OS works for any domain using a consistent 4-tier model:

| Tier | Role | Importance | `linksIn` |
|---|---|---|---|
| **0 · Root** | The central thesis, product, or entity | 100 | `[]` |
| **1 · Components** | Major pillars or subsystems | 60–95 | `[root_id]` |
| **2 · Processes** | Transformations, methods, workflows | 40–92 | `[component_id]` |
| **3 · Atoms** | Irreducible elements — ingredients, people, tasks, sources | 1–100 | `[process_id]` |

This maps to any domain:

```
Recipe    →  dish       → ingredient group  → cooking method  → ingredient
Org       →  company    → department        → workflow        → person / role
Project   →  goal       → milestone         → task            → asset / file
System    →  product    → module            → function        → dependency
Essay     →  thesis     → argument          → evidence        → source
```

---

## Changelog

### v1.041 *(current)*
- Annotated source with universal schema documentation embedded as comments
- `_schema` top-level key silently ignored on import — use it to embed diagram rules in JSON
- Per-view state memory: each view mode (Network, 3D, Importance Rings) remembers its last zoom and camera position independently
- Node-position snapshots saved on view departure, restored on return
- Auto zoom-fit on load respects pre-positioned JSON coordinates
- Mobile layout: hamburger drawer, panel FAB, double-tap to edit
- Filter bar: type chips, status dropdown, tag text input, orphan filter
- Quick Switcher overlay (Ctrl+K)
- Stats panel with pie charts and graph metrics
- Tags panel with tag cloud
- Animated link particles with pause/resume
- Importance Rings view mode
- `linkMeta` per-edge label support

### v1.033
- Initial public release
- Force-directed network with D3.js
- 3D WebGL view
- Properties / Index side panel
- Save as JSON, SVG, and localStorage
- Drag & drop JSON import

---

## License

For personal and internal use. Not licensed for commercial redistribution.

---

*ECO.SYSTEM.OS — GOVERNANCE MAPPER · © Constantinos Tolias 2026*
