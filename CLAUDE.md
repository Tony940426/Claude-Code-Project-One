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

## Additional Documentation

Check these files when working on related concerns:

- [`.claude/docs/architectural_patterns.md`](.claude/docs/architectural_patterns.md) — data layer, state management, rendering model, event handling conventions
