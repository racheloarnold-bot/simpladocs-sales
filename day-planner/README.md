# SimplaDocs Day Planner

Branded interactive day planner for Rachel Arnold, Head of Sales at SimplaDocs.

## Files

- `simpladocs-day-plan.html` — Single-file interactive day planner. Self-contained HTML/CSS/JS with time-blocked schedule, checkboxes, drag-and-drop, water tracker, HubSpot tasks tab, Q2 Rocks tab, and Notes tab.

## Usage

The `/day-planner` Claude skill fetches this file fresh each morning, customizes it for the current day (BLOCKS array, localStorage keys, pre-checked tasks), writes it to `~/simpladocs-day-plan.html`, and opens it in the browser.

## Structure

The JS `const BLOCKS = [...]` array is the source of truth for the day's schedule. The skill updates this array for each new day along with date-specific localStorage keys (`sd-today-mmm#-v1`).

## Features

- Time-blocked schedule with drag-and-drop reordering
- Task checkboxes with localStorage persistence
- HubSpot Tasks tab (manually synced)
- Q2 Rocks tab with progress tracking
- Notes / 1:1 prep tab
- Water tracker
- Cherry blossom background animation
- High-priority task highlighting (purple ⚡)
- Meds reminder highlighting (lime green 💊)
