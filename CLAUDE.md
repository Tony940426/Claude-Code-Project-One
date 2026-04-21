# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ChoreBoard is a single-page household chore tracker with a monthly calendar view. Users can create chores with recurrence rules, assign them to team members, and log completions with history.

## Tech Stack

- **Vanilla HTML/CSS/JS** — no frameworks, no build tools, no dependencies
- **localStorage** — sole persistence layer (no backend, no server)
- Single file: `chores.html` contains all markup, styles, and logic

## Key Directories

```
chores.html        — entire application (HTML + CSS + JS in one file)
.claude/docs/      — supplementary documentation for Claude
```

## Build / Run

No build step. Open `chores.html` directly in a browser:

```
# macOS / Linux
open chores.html

# Windows
start chores.html
```

No tests exist at this time.

## Key Behaviors

- **Cross-tab sync**: mutations propagate to other open tabs automatically via a `window` `storage` event listener in section 10 of `chores.html`. When any tab writes to `chores_app_chores`, `chores_app_members`, or `chores_app_completions`, every other same-origin tab re-renders (and refreshes the Team modal list if it happens to be open). Scope: tabs in the same browser on one device — this is *not* a multi-user / multi-device feature.
- **New chores cannot start in the past**: the Add Chore date picker's `min` is set to today and `validateAndSaveChore` rejects past start dates. Editing an existing chore is unaffected — `min` is removed in edit mode, so chores with past start dates still work.
- **End date must be ≥ start date**: the end date input's `min` tracks the start date input (both on modal open and via a `change` listener), and save is blocked if end < start.

## Additional Documentation

Check these files when working on related concerns:

- [`.claude/docs/architectural_patterns.md`](.claude/docs/architectural_patterns.md) — data layer, state management, rendering model, event handling conventions
