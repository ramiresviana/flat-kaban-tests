# Phase 6: Task Details Modal

> Status: ready

## Goal
Modal panel that opens when a task card or row is clicked. Allows editing title, tags, and body text. All edits sync back to the task's `raw` block and trigger auto-save.

## State Additions
```js
selectedTask: null,       // Task | null
modalOpen: false,
newTagInput: '',          // tag being typed in the add-tag field
```

## Open / Close
```js
openTask(task)   // selectedTask = task; modalOpen = true; focus modal
closeTask()      // modalOpen = false; selectedTask = null
```

## Edit Handlers
```js
updateTitle(task, newTitle)
// 1. Remove existing #title(...) from raw
// 2. If newTitle: prepend #title(newTitle)\n to rebuilt raw
// 3. Re-parse task.title
// 4. autosave()

updateTags(task, newTags)    // replaces tag set; rebuilds managed header
addTag(task, tag)            // calls updateTags with tag appended
removeTag(task, tag)         // calls updateTags with tag removed

updateBody(task, newBody)    // rebuild raw = header + newBody; autosave()
```

### buildTaskRaw(title, tags, body) (reused from SERIALIZER)
```
#title(T)          ← only if title is set
#tag1 #tag2 …      ← only if tags.length > 0
<body>             ← verbatim
```

### extractBody(raw)
Filters lines that are purely managed (title line or all-tag line) to return only user prose.

## Modal HTML
```html
<dialog role="dialog" aria-modal="true" aria-labelledby="modal-title"
        x-show="modalOpen" @keydown.escape.window="closeTask()">
  <header>
    <input id="modal-title" x-model="selectedTask.title"
           @change="updateTitle(selectedTask, $event.target.value)">
    <button @click="closeTask()">✕</button>
  </header>
  <!-- Tags row -->
  <div class="tag-editor">
    <span x-for="tag in selectedTask.tags">
      {{ tag }} <button @click="removeTag(selectedTask, tag)">✕</button>
    </span>
    <input placeholder="Add tag…" x-model="newTagInput"
           @keydown.enter.prevent="addTag(selectedTask, newTagInput); newTagInput=''">
  </div>
  <!-- Body -->
  <textarea x-model="bodyText"
            @input.debounce.500ms="updateBody(selectedTask, $event.target.value)"></textarea>
</dialog>
<div class="modal-backdrop" x-show="modalOpen" @click="closeTask()"></div>
```

## Autosave
```js
autosave()  // debounced 400 ms; calls saveToStorage(serializeDatabase(tasks, preferences))
```

## Accessibility
- `<dialog>` uses native focus trapping (or manual trap for older browsers)
- Escape key closes modal
- Tab order: title → tag chips → add-tag input → body → close

## Commit
`feat(modal): task details panel with title, tag, and body editing`
