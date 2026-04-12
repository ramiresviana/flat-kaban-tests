# Phase 1: Scaffold & Shell

> Status: ready

## Goal
Create `index.html` with the full HTML skeleton, CDN dependencies, CSS custom-property foundation, and an Alpine.js root component stub with routing state only.

## Deliverables
- `index.html` with:
  - `<head>`: charset, viewport, title, CDN links
  - `<style>`: CSS custom properties (color tokens, spacing scale) for light and dark schemes; reset; layout grid
  - `<body x-data="kanbanApp()">`: header + `<main>` placeholder
  - `<script>`: `kanbanApp()` stub returning `{ currentView: 'kanban' }` with empty `init()`
- Header contains: app logo/title, view-tab buttons (Kanban / List / Raw), theme-toggle icon button

## CDN Dependencies
| Library | URL | Purpose |
|---------|-----|---------|
| Alpine.js v3 | `https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js` | Lightweight reactivity (< 15 KB gzip) |
| Inter font | Google Fonts | Clean modern UI font |
| Feather Icons | `https://cdn.jsdelivr.net/npm/feather-icons/dist/feather.min.js` | Crisp 24px icon set (SVG sprite) |

**Rationale for Alpine.js**: Zero-build, CDN-first, < 15 KB gzip. Replaces boilerplate reactive getters. Vue/React would require a build step; vanillaJS event wiring would grow unwieldy for this feature set.

## CSS Architecture
- `:root` holds all design tokens: `--bg`, `--surface`, `--border`, `--text`, `--accent`
- `[data-theme=dark]` overrides those tokens
- `@media (prefers-color-scheme: dark)` applied when theme is `auto`
- Spacing scale: `--space-1` … `--space-6` (0.25rem increments)
- Layout: `#app` flex-column full-height; `header` fixed height; `main` `flex: 1; overflow: auto`

## Alpine Root Stub
```js
function kanbanApp() {
  return {
    currentView: 'kanban',  // 'kanban' | 'list' | 'raw'
    init() {},
  };
}
```

## Commit
`feat(scaffold): initial HTML shell with CDN deps and Alpine stub`
