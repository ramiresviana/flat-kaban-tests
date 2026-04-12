# Phase 3: Theme & Header UI

> Status: ready

## Goal
Wire up Alpine.js state for theme, tag-filter, and search; render the header controls; apply dynamic theme attribute; show placeholder content areas per view.

## Alpine State Additions
```js
theme: 'auto',          // restored from preferences
tagFilter: 'all',       // 'none' | 'all' | <tag>
searchQuery: '',
tasks: [],
preferences: {},
rawText: '',
```

## Computed Getters
```js
get allTags()       // unique sorted tags from all tasks
get visibleTasks()  // tasks filtered by tagFilter + searchQuery
```

### Tag Filter Logic
- `none` → all tasks (no tag filtering)
- `all` → tasks that have at least one tag
- `<tag>` → tasks that include that exact tag
- Then further filtered by `searchQuery` (case-insensitive match on raw block text)

## Header HTML
```
[logo/title]  [Kanban][List][Raw]  [tag-select][search-input]  [theme-toggle]
```
- View tabs: `<button>` with `x-bind:class="{ active: currentView === 'kanban' }"`
- Tag selector: `<select>` with dynamic `<option>` list built from `allTags`
- Search: `<input type="search" x-model="searchQuery">`
- Theme toggle: Feather icon button cycling `auto→light→dark→auto`

## Theme Mechanics
- `<html>` tag receives `data-theme` attribute managed by Alpine effect
- CSS: `:root` = light tokens; `[data-theme=dark]` = dark overrides
- When `theme === 'auto'`, no attribute set → CSS media query handles it
- Toggle button shows sun/moon/circle icon for light/dark/auto

## init() Wiring
```js
init() {
  this.rawText = loadFromStorage();
  const { tasks, preferences } = parseDatabase(this.rawText);
  this.tasks = tasks;
  this.preferences = preferences;
  this.theme = preferences.theme ?? 'auto';
  this.tagFilter = preferences.tagFilter ?? 'all';
  this.$watch('theme', () => applyTheme(this.theme));
  applyTheme(this.theme);
}
```

## Commit
`feat(theme): header controls, theme toggle, tag-filter, and search wiring`
