# Plain Kanban

A single-file, offline-capable, local-first Kanban app that runs entirely in the browser.  
No build step, no server, no npm — just open `index.html`.

---

## Getting Started

1. Clone or download this repository.
2. Open `index.html` in any modern browser (Chrome 90+, Firefox 98+, Safari 15.4+).
3. On first load a demo dataset is shown automatically. Start editing right away.
4. The app works fully offline after the first load (CDN assets are cached by the browser).

---

## Views

| View | Description |
|------|-------------|
| **Kanban** | Board with one column per tag. Drag cards between columns to reassign tags. |
| **List** | Sortable table of all tasks. Optional tag-based grouping with collapsible sections. |
| **Raw** | Full plain-text editor for the entire database. Save manually with the **Save** button. |

---

## Header Controls

- **Column / Tag filter** — `None` shows untagged tasks; `All tags` creates one column per tag; picking a specific tag filters to that tag.
- **Search** — Live text search across all task content.
- **Export** (↓) — Downloads the current database as `plain-kanban.txt`.
- **Import** (↑) — Uploads a `.txt` file to replace the current database.
- **Theme toggle** — Cycles between Auto (follows OS), Light, and Dark mode.

---

## Data Format

The database is a single plain-text file. Each task is a block of free text separated by a line containing exactly `---`:

```
First task content goes here.
You can write anything.
---
#title(Titled Task)
#design #ux
This task has an explicit title and two tags.
---
Another task — no title, inferred from first words.
#backend
```

### Syntax tokens (anywhere in a task block)

| Token | Purpose |
|-------|---------|
| `#title(My Title)` | Sets the display title of the task. |
| `#tagname` | Attaches a tag (lowercase, alphanumeric + `_-`). |

- Tags are normalized to lowercase.
- If no `#title(...)` is present, the UI infers a title from the first words of the block.
- When the app adds/updates metadata programmatically, it inserts `#title(...)` and tag lines at the **top** of the block.

### Preferences section

At the end of the file, after the marker line `---PREFERENCES---`, the app stores UI state as `key=value` lines:

```
---PREFERENCES---
theme=auto
tagFilter=all
listGroupBy=none
listSortBy=title
listSortDir=asc
```

| Key | Values |
|-----|--------|
| `theme` | `auto` \| `light` \| `dark` |
| `tagFilter` | `none` \| `all` \| _tag name_ |
| `listGroupBy` | `none` \| _tag name_ |
| `listSortBy` | `title` \| `tagCount` |
| `listSortDir` | `asc` \| `desc` |

---

## Persistence & Data Safety

- **Auto-save** — UI edits (title, tags, body, drag-drop) are saved to `localStorage` automatically (400 ms debounce).
- **Raw view** — Changes require an explicit **Save** click.
- **Export** — Download the database as a `.txt` file for backup or portability.
- **Import** — Load a previously exported `.txt` file.
- **localStorage key**: `pk_db`

> Tip: export regularly to keep a backup, since `localStorage` can be cleared by the browser.

---

## Example File

See [`example.txt`](example.txt) for a sample database with multiple tasks and a preferences section that you can import directly.

---

## CDN Dependencies

| Library | Version | Size (gzip) | Purpose |
|---------|---------|-------------|---------|
| [Alpine.js](https://alpinejs.dev/) | v3.14.9 | ~15 KB | Lightweight DOM reactivity — no build step required |
| [Inter](https://fonts.google.com/specimen/Inter) | — | ~10 KB | Clean sans-serif typeface |
| Feather Icons | 4.29.2 | — | SVG icon set (inline, no external requests after first load) |

---

## Offline Use

After the first page load all CDN resources are cached by the browser. Subsequent loads work without a network connection provided the browser cache has not been cleared.

---

## License

MIT
