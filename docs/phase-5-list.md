# Phase 5: List View

> Status: ready

## Goal
Render tasks as a sortable table. Support optional grouping by a tag, with collapsible groups. Columns: Title, Snippet, Tags.

## State Additions
```js
listGroupBy: 'none',    // tag name or 'none'
listSortBy: 'title',    // 'title' | 'tagCount'
listSortDir: 'asc',     // 'asc' | 'desc'
```

## Computed
```js
get sortedTasks()   // visibleTasks sorted by listSortBy/Dir
get groupedList()   // [{ group: string, tasks: Task[] }]
                    // If listGroupBy === 'none' → single group ''
                    // Otherwise one group per tag value + 'Untagged'
```

## Sort Toggle
```js
togglesSort(col)  // if same col → flip dir; else set col + reset to 'asc'
```

## Group Collapse
```js
collapsedGroups: new Set(),
toggleGroup(group)  // add/remove from collapsedGroups
```

## HTML Structure
```html
<div class="list-controls">
  Group by: <select x-model="listGroupBy">…</select>
</div>
<template x-for="group in groupedList">
  <section>
    <!-- group header (click to collapse) -->
    <table>
      <thead>
        <tr>
          <th @click="toggleSort('title')">Title <icon/></th>
          <th>Snippet</th>
          <th @click="toggleSort('tagCount')">Tags <icon/></th>
        </tr>
      </thead>
      <tbody x-show="!collapsedGroups.has(group.group)">
        <template x-for="task in group.tasks">
          <tr @click="openTask(task)">…</tr>
        </template>
      </tbody>
    </table>
  </section>
</template>
```

## Commit
`feat(list-view): sortable table with tag grouping and collapsible sections`
