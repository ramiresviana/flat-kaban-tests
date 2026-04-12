# Phase 7: Raw View

> Status: ready

## Goal
Full-screen editable textarea showing the complete raw database text. Changes are not applied until the user clicks **Save**. Shows an unsaved-changes indicator.

## State Additions
```js
rawEdited: false,   // true when textarea content differs from stored rawText
```

## HTML Structure
```html
<section x-show="currentView === 'raw'" class="raw-view">
  <div class="raw-toolbar">
    <span x-show="rawEdited" class="unsaved-badge">Unsaved changes</span>
    <button @click="saveRaw()" :disabled="!rawEdited">Save</button>
    <button @click="discardRaw()">Discard</button>
  </div>
  <textarea class="raw-editor"
            x-model="rawDraft"
            @input="rawEdited = rawDraft !== rawText"
            spellcheck="false"
            autocomplete="off"></textarea>
</section>
```

## State
```js
rawDraft: '',       // bound to textarea; initialised = rawText each time view opens
```
When switching TO raw view: `rawDraft = rawText; rawEdited = false`.

## Actions
```js
saveRaw()
// 1. rawText = rawDraft
// 2. Re-parse: { tasks, preferences } = parseDatabase(rawText)
// 3. saveToStorage(rawText)
// 4. rawEdited = false

discardRaw()
// rawDraft = rawText; rawEdited = false
```

## Commit
`feat(raw-view): full-text editor with explicit save and discard`
