# Phase 2: Parser & Data Model

> Status: ready

## Goal
Implement the plain-text database format: parser (text→model), serializer (model→text), localStorage layer, and demo seed data.

## Data Format Spec
```
<task block>\n---\n<task block>\n---\n...\n---PREFERENCES---\nkey=value\n...
```
- Tasks separated by `\n---\n` (three dashes on their own line)
- Preferences section begins at line `---PREFERENCES---` (distinct marker)
- Within a task block:
  - `#title(Title text)` — explicit title; parsed anywhere
  - `#tagname` — inline tag token; parsed anywhere (case-insensitive, normalized to lowercase)
  - A line whose every space-separated token is `#word` is a **managed tag line** (produced by the app)
  - All other content is preserved verbatim

## Data Model
```ts
interface Task {
  id: number;       // sequential, stable within session
  raw: string;      // original block text (preserved)
  title: string | null;   // from #title(...) or null
  tags: string[];   // normalized, unique
}

interface Preferences {
  theme: 'auto' | 'light' | 'dark';
  tagFilter: 'none' | 'all' | string;
  listGroupBy: string;  // tag name or 'none'
  listSortBy: 'title' | 'tagCount';
  listSortDir: 'asc' | 'desc';
}
```

## Parser Functions (PARSER section)
```js
// parseDatabase(text) → { tasks, preferences }
// parseTask(block, id) → Task
// extractTags(block) → string[]
// parsePreferences(text) → Preferences
```

## Serializer Functions (SERIALIZER section)
```js
// serializeDatabase(tasks, preferences) → string
// buildTaskRaw(title, tags, body) → string  ← used when syncing edits
// extractBody(raw) → string                 ← strips managed header lines
// serializePreferences(prefs) → string
```

### Round-trip Safety
- `serializeDatabase(parseDatabase(text).tasks, parseDatabase(text).preferences) === text` for any unedited input.
- The serializer joins task `raw` values with `\n---\n` — it never reprocesses the raw string.
- Only `buildTaskRaw` rewrites a task block; called only after a UI edit.

## Storage Functions (STORAGE section)
```js
// loadFromStorage() → string   (returns raw DB text or DEMO_DATA)
// saveToStorage(text)           (writes to localStorage key 'pk_db')
```

## Demo Seed
A multiline constant `DEMO_DATA` with ~6 tasks covering at least 4 tags.

## Commit
`feat(parser): plain-text DB parser, serializer, localStorage, and demo seed`
