# Phase 4: Kanban View

> Status: ready

## Goal
Render a Kanban board where each column corresponds to a tag. Tasks with multiple matching column tags appear in each matched column. Drag-and-drop moves a task to a new column tag (adds new tag, removes old column tag).

## Column Logic
```js
get columns() {
  // If tagFilter === 'none' → single column 'Untagged' (tasks with no tags)
  // If tagFilter === 'all'  → one column per unique tag across all tasks (sorted)
  // If tagFilter === <tag>  → single column for that tag
  // Returns: [{ tag, tasks[] }]
}
```

## Task Card HTML
```html
<div class="card"
     draggable="true"
     @dragstart="dragStart($event, task)"
     @click="openTask(task)">
  <p class="card-title">{{ displayTitle(task) }}</p>
  <p class="card-snippet">{{ snippet(task) }}</p>
  <div class="card-tags">
    <span x-for="tag in task.tags" class="tag-chip">#{{ tag }}</span>
  </div>
</div>
```

## Drag-and-Drop
```js
// State
draggedTask: null,
dragSourceColumn: null,

// Handlers
dragStart(event, task, sourceTag)
// sets draggedTask; stores sourceTag

dropOnColumn(event, targetTag)
// 1. If targetTag === dragSourceColumn → no-op
// 2. Remove dragSourceColumn from draggedTask.tags
// 3. Add targetTag to draggedTask.tags (if not already present)
// 4. Rebuild raw for that task via buildTaskRaw()
// 5. saveDatabase()
```
- Column drop zones: `@dragover.prevent` + `@drop="dropOnColumn($event, col.tag)"`
- Visual feedback: `dragover` CSS class on hovered column

## displayTitle(task) helper
Returns `task.title` if set, else first 8 words of `task.raw` stripped of metadata tokens.

## snippet(task) helper
Returns first 20 words of the body (non-metadata content), truncated with `…`.

## Commit
`feat(kanban): board columns, task cards, and drag-and-drop tag update`
