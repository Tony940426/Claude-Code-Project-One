# Architectural Patterns

All code lives in `chores.html`. Sections are marked with numbered banners (`// ── 1. CONSTANTS ──` through `// ── 10. INIT ──`) which define the logical layering.

## Data Layer (localStorage abstraction)

All persistence goes through two primitives (`chores.html:652-663`):

```
load(key)  → JSON.parse(localStorage.getItem(key)) || []
save(key, arr) → localStorage.setItem(key, JSON.stringify(arr))
```

Higher-level helpers (`loadChores`, `saveChores`, `loadMembers`, etc.) wrap these. **Never call `localStorage` directly** — always go through the typed helpers. Three independent stores: `MEMBERS`, `CHORES`, `COMPLETIONS` (keys defined at `chores.html:640-644`).

## Centralized State Object

A single mutable `state` object (`chores.html:892-897`) holds ephemeral UI state:

- `currentYear` / `currentMonth` — drives which month is rendered
- `editingChore` — the chore being edited in the modal (null = new)
- `detailContext` — `{ chore, dateStr }` for the detail/history modal

All modal controllers read from and write to `state` rather than passing arguments through the call chain.

## Render-on-Mutate Convention

Every function that mutates persistent data ends by calling `renderCalendar(state.currentYear, state.currentMonth)`. There is no reactive/observable system — rendering is triggered imperatively at each mutation site (`chores.html:764`, `783`, `1025`).

## Event Delegation

All click events are handled by a single `document.body` listener (`chores.html:1120`) that pattern-matches on `e.target` using `t.matches(selector)` and `t.closest(selector)`. Adding a new interactive element means adding a branch here, not attaching a listener to the element itself.

## Recurrence Engine

`generateOccurrences(chore, year, month)` (`chores.html:687`) is a pure function — given a chore definition and a year/month, it returns an array of ISO date strings when that chore occurs. It supports four types stored in `chore.recurrence.type`: `once`, `daily`, `weekly`, `monthly`. The calendar renderer calls this per-chore per-cell.

## Completion Records

Completions are stored separately from chores as an append-only log (`COMPLETIONS` store). Each record: `{ choreId, occurrenceDate, completedByName, completedAt }`. Deleting a chore cascades to delete its completions (`chores.html:768-770`). There is no edit path for completions — only mark/unmark.
