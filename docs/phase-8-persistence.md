# Phase 8: Export/Import & README

> Status: ready

## Goal
Export database as `.txt` download; import from file upload; wire preferences persistence; write README and example DB.

## Export
```js
exportFile()
// 1. Blob from rawText; MIME text/plain
// 2. Create temporary <a download="plain-kanban.txt"> and click
// 3. Revoke object URL
```

## Import
```js
importFile(event)
// 1. Read FileReader from event.target.files[0]
// 2. On load: rawText = result; re-parse; saveToStorage(rawText)
// 3. Reset file input value
```

## Preferences Persistence
```js
savePreferences()
// Serialize updated prefs into rawText, save to storage.
// Called after theme change, tagFilter change, sort/group changes.
```

## Header Export/Import Buttons
Small icon-buttons in the header bar:
- Export: `feather-download` icon
- Import: `feather-upload` icon (hidden `<input type=file>` triggered via JS)

## Accessibility Final Pass
- All interactive elements have visible focus rings
- Drag-and-drop has keyboard alternative: task detail modal tag editing
- `role="status"` live region for "Saved" / "Error" toast
- `aria-label` on icon-only buttons

## README
- Project description
- How to open offline (just open index.html)
- Data format spec summary
- CDN dependencies list
- Export/import usage
- First-run demo data description

## Example DB File
`example.txt` — a standalone file with ~6 tasks and preferences section, usable as an import file.

## Commit
`feat(persistence): export/import, preferences sync, accessibility, README`
