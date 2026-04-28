# ECO.SYSTEM.OS

**Governance Network Mapper** · v1.050

> A single-file, browser-based tool for mapping and visualising governance relationships — policies, standards, regulations, roles, systems, risks, controls, assets, and objectives — as an interactive force-directed network.

---

## What it is

ECO.SYSTEM.OS is a self-contained HTML application. There is no server, no build step, no login. Open the file in a browser and start mapping.

It was built for practitioners who need to see governance as a system — not as a document. The graph reveals structural gaps, orphaned nodes, over-centralised roles, and missing controls that lists and spreadsheets never surface.

---

## Features

| Capability | Detail |
|---|---|
| **Force-directed graph** | D3.js-powered, physics-simulated node layout |
| **Node types** | Policy, Standard, Regulation, Role, System, Risk, Control, Asset, Objective, and custom types |
| **Node properties** | Label, type, status, importance (0–100), description, tags, owners, custom fields |
| **Visual encoding** | Node size scales with importance; colour encodes type and status |
| **Relationship arrows** | Per-link coloured SVG markers with metadata support |
| **Type Clusters view** | Group nodes spatially by type |
| **3D force graph** | Toggle to full 3D canvas (three.js via `3d-force-graph`) |
| **Timeline Arc view** | Temporal layout mode |
| **XLSX import** | Bulk-load nodes from a spreadsheet (SheetJS) |
| **PNG export** | 2× pixel density canvas snapshot |
| **Persistent storage** | Supabase (cloud) + localStorage (local) with auto-save |
| **URL-addressable maps** | Every map gets a stable `?mapId=` URL |
| **Embed mode** | Read-only `?embed=1` iframe-safe rendering |
| **Edit-key gate** | Optional `?edit=<key>` write protection per map |
| **Single-file architecture** | Everything in one `.html` — no dependencies to install |

---

## Quick start

1. Download `index.html`
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge)
3. Click **+** to add your first node
4. Double-click any node to edit its properties
5. Drag nodes to arrange the layout

No internet connection required for local use.

---

## Persistence & sharing (optional)

By default the app saves to `localStorage` only — your map lives in the browser.

To enable cloud persistence, URL sharing, and embeds, connect a free [Supabase](https://supabase.com) project.

### 1 · Create the database table

Run this once in the Supabase SQL editor:

```sql
create table if not exists maps (
  id          text primary key,
  data        jsonb not null,
  edit_key    text,
  created_at  timestamptz default now(),
  updated_at  timestamptz default now()
);

alter table maps enable row level security;
create policy "public read"   on maps for select using (true);
create policy "public upsert" on maps for insert with check (true);
create policy "public update" on maps for update using (true);
```

### 2 · Add your project credentials

Open `index.html` and find these two lines near the bottom of the `<script>` block:

```js
const ECO_SUPABASE_URL  = '';   // → Settings → API → Project URL
const ECO_SUPABASE_ANON = '';   // → Settings → API → anon / public key
```

Paste your values. The anon key is safe to include — Supabase row-level security controls access.

### 3 · Done

Auto-save begins immediately. Every change is debounced (1.2 s) and written to Supabase. The URL updates to include a stable `?mapId=` parameter you can share directly.

---

## URL patterns

| URL | Behaviour |
|---|---|
| `index.html` | Creates a new map with a generated ID; begins auto-saving |
| `index.html?mapId=map_abc123` | Loads the exact saved layout; resumes auto-saving |
| `index.html?mapId=map_abc123&embed=1` | Read-only, no chrome — safe for `<iframe>` embedding |
| `index.html?mapId=map_abc123&edit=mykey` | Enables editing only when the key matches `edit_key` in the database |

---

## Embedding

```html
<iframe
  src="index.html?mapId=YOUR_MAP_ID&embed=1"
  width="100%"
  height="600"
  style="border:none;">
</iframe>
```

The layout is fully deterministic in embed mode — node positions are stored explicitly (no re-simulation on load).

---

## State model

The canonical state object saved to Supabase and localStorage:

```json
{
  "version": "1.050",
  "nodes": [
    { "id": "…", "label": "…", "type": "…", "status": "…", "importance": 50, "x": 120, "y": -84 }
  ],
  "links": [],
  "customTypes": [],
  "customStatuses": [],
  "viewport": { "k": 1, "x": 0, "y": 0 },
  "metadata": { "savedAt": "2026-…", "nodeCount": 12 }
}
```

`x` and `y` are always stored explicitly. Layout is never recalculated from scratch on load — the simulation runs at `alpha: 0.08` to settle immediately without drift.

---

## Architecture

ECO.SYSTEM.OS is intentionally a **single HTML file**. All logic — graph engine, UI, persistence — lives in one document. There is no build toolchain, no package manager, no framework.

External libraries are loaded from CDN:

| Library | Purpose |
|---|---|
| [D3.js v7](https://d3js.org) | Force-directed graph, zoom, drag |
| [3d-force-graph](https://github.com/vasturiano/3d-force-graph) | 3D canvas view |
| [SheetJS](https://sheetjs.com) | XLSX import |
| [GSAP](https://gsap.com) | UI animations |
| [Supabase REST API](https://supabase.com/docs/guides/api) | Cloud persistence (optional) |

Fonts: [Syne](https://fonts.google.com/specimen/Syne) · [DM Mono](https://fonts.google.com/specimen/DM+Mono) · [Sunflower](https://fonts.google.com/specimen/Sunflower) via Google Fonts.

---

## Design

The visual language is Swiss editorial — dark canvas (`#0a0a0a`), electric lime accent (`#c8ff00`), tight typographic hierarchy. The UI chrome is deliberately minimal so the graph commands full attention.

Inspired by: Pacifica / Harry Atkins · Flowsint · Awwwards editorial direction.

---

## Keyboard & interaction reference

| Interaction | Action |
|---|---|
| Click node | Select |
| Double-click node | Open property editor |
| Right-click node | Open property editor |
| Drag node | Reposition (pins on release) |
| Click canvas | Deselect |
| Scroll / pinch | Zoom |
| Drag canvas | Pan |
| Double-tap (mobile) | Open property editor |

---

## Licence

© 2026 Constantinos Tolias. ECO.SYSTEM.OS

Licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) — free for personal and non-commercial use.
Commercial use by organisations requires written permission from the author.

---

## Author

**Constantinos Tolias**
ECO.SYSTEM.OS is an independent project.
