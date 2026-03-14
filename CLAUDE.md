# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the app

Open `index.html` directly in a browser — no build step, no server, no dependencies. There is no package.json.

## Architecture

Single-file app: all HTML, CSS, and JS live in `index.html`. No framework, no modules.

### JS structure (in order of appearance)

| Section | What it does |
|---------|-------------|
| **Constants** | `HOUR_W`, `DAY_W`, `PPM`, `ROW_H`, `SIDEBAR_W`, `SNAP` — all pixel/time math derives from these |
| **Utilities** | Date helpers, coordinate converters (`dateToX`, `xToDate`, `viewportXToAreaX`), `esc()`, `uid()` |
| **Data** | `RESOURCES` (const array), `ACTIVITIES`/`COLORS` (const), `events` (let — mutable) |
| **State** | `state` object for week navigation + modal state; `drag` module-level variable for drag operations |
| **Render** | `render()` calls `renderPlanner()` + `renderModal()` — full innerHTML replacement each time, no diffing |
| **Drag** | Three types: `create` (ghost block), `move` (fixed-position float across rows), `resize` (edge handles) |
| **Modal** | `openModal(evt, isNew)` / `closeModal()` — re-renders only `#modal-root` |

### Coordinate system

All event positions are stored as JavaScript `Date` objects. On render, they are converted to pixel X offsets within `.timeline-area` via `dateToX(date, weekStart)`. The inverse is `xToDate(px, weekStart)`.

During a **move drag**, the block is lifted to `position:fixed` on `document.body` to escape `overflow:hidden` of the scroll container. On `mouseup`, `viewportXToAreaX(clientX)` converts the final viewport position back to a timeline-relative pixel, then `xToDate` produces the new `Date`.

### Key layout rules

- `.planner-scroll` is the single scroll container (both X and Y).
- `.resource-info` cells use `position:sticky; left:0` — they stay visible on horizontal scroll.
- `.planner-header` uses `position:sticky; top:0` — stays visible on vertical scroll.
- Events are `position:absolute` children of `.timeline-area`, sized with `left` + `width` in pixels.

### Data model

```js
// Event
{ id, title, description, activity, start: Date, end: Date, allDay, resourceIds: [id], order }

// Resource
{ id, name, role, external }
```

`resourceIds` is an array but the UI currently assigns exactly one resource per event.

## Extending the app

- **Add a resource:** append to the `RESOURCES` array.
- **Add an activity/color:** add to `ACTIVITIES` array and `COLORS` object.
- **Add an order:** append to the `ORDERS` array.
- **Persistence:** `events` is a plain `let` array — wrap saves/loads around it (localStorage or fetch to an API).
- **Backend:** the app is designed to swap the in-memory `events` array for `fetch('/api/events')` calls. No other structural change is needed.

## Screenshots / reference

`docs/screenshots/` contains the original design references (`Planner.png`, `Event erstellen.png`).
