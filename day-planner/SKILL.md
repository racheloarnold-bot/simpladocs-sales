---
name: day-planner
description: "Interactive daily planning workflow: brain-dump → prioritize → schedule → calendar → checklist. Use this skill whenever the user wants to plan their day, organize tasks, do a morning planning session, figure out what to work on, or says things like \"let's plan my day,\" \"what's on my plate,\" \"morning planning,\" \"help me organize today,\" \"what should I do today,\" \"daily standup,\" \"let's get organized,\" or references a brain-dump of tasks they need to sort through. Also trigger when the user asks to update, modify, or rebuild their day plan or checklist, or when they share a screenshot of their existing checklist and want it refreshed. Do NOT use for long-term project planning, weekly/monthly reviews, or goal-setting that isn't about today's specific schedule."
---

# Day Planner

An interactive daily planning workflow that moves from brain-dump through prioritization to a time-blocked interactive checklist.

## HTML Template Source

The day planner is a single-file HTML app. The canonical template lives on GitHub:

**Repo:** `racheloarnold-bot/simpladocs-sales`
**File:** `day-planner/simpladocs-day-plan.html`
**Raw URL:** `https://raw.githubusercontent.com/racheloarnold-bot/simpladocs-sales/main/day-planner/simpladocs-day-plan.html`

### Fetching and customizing for a new day

When building the day planner, always fetch the template fresh from GitHub rather than using a local file:

```bash
curl -sL "https://raw.githubusercontent.com/racheloarnold-bot/simpladocs-sales/main/day-planner/simpladocs-day-plan.html" -o ~/simpladocs-day-plan.html
```

Then customize it via Python (binary mode to avoid encoding issues) with these changes:

1. **localStorage keys** — bump the date suffix: `'sd-today-apr8-v1'` → `'sd-today-apr9-v1'` (use abbreviated month + day number)
2. **BLOCKS array** — replace the entire `const BLOCKS = [...]` with today's schedule (find array boundaries by tracking `[`/`]` depth)
3. **Meds CSS** — already present in template (lime green, `.meds-row`)
4. **High-priority CSS** — already present in template (purple, `.hi-row`)
5. **Pre-checks** — inject checked state for any already-completed tasks after the `todayState = {...}` line
6. **HubSpot Tasks tab** — update from fresh HubSpot pull
7. **Q2 Rocks tab** — update from `~/Projects/MarkdownBrain/sales/q2-rocks.md`

Write to `~/simpladocs-day-plan.html` and `~/Desktop/simpladocs-day-plan.html` using **binary mode** (`'rb'`/`'wb'`) — never text mode, which causes encoding corruption.

### BLOCKS array format

```javascript
const BLOCKS = [
  { id:'blockid', title:'Block Title', start:9.0, end:10.5, tasks:[
    { id:'t-taskid', text:'Task text', note:'Optional note' },
    { id:'t-taskid2', hi:true, text:'High priority task', note:'Note' },
    { id:'t-taskid3', meds:true, text:'💊 Take meds', note:'Reminder note' },
  ]},
  { id:'break', title:'Lunch', start:12.0, end:13.0, tasks:[], isBreak:true },
];
```

- `hi:true` — purple highlight with ⚡, use for high-priority tasks
- `meds:true` — lime green highlight, use for meds reminders
- `isBreak:true` — renders as a break card (no task list)
- `start`/`end` — decimal hours (e.g. 9.75 = 9:45 AM, 13.5 = 1:30 PM)

### Auto-refreshing the HubSpot Tasks tab

Every time the day planner is built or rebuilt, fetch fresh HubSpot tasks and replace the static tab. Do this via the HubSpot MCP:

1. Fetch open tasks assigned to Rachel (owner ID `89333594`, `hs_task_status != COMPLETED`)
2. Build the full `<div id="tab-hubspot">` block with today's tasks as `<li>` items, grouped by due date if helpful
3. Replace the existing `<div id="tab-hubspot">...</div>` in the template before writing the file

This replaces the need to manually ask "can you refresh my HubSpot tasks" — it always happens automatically.

### Capture notes bridge

The day planner's Notes tab stores capture notes in browser localStorage (`id="notes-general"`). To bring them into a planning session:

1. At the **start of every planning session**, prompt Rachel: *"Before we start — click the 'Copy capture notes' button in your day planner's Notes tab, then paste anything you want me to factor in."*
2. The button (purple, top-right of the Capture textarea) copies the notes to clipboard
3. If Rachel pastes content, treat it as brain-dump input alongside whatever she types — factor it into prioritization and scheduling
4. If Rachel says "nothing" or skips, proceed without it

### After updating, push changes back to GitHub

When the day planner is finalized (end of planning session or end of day), push the updated file back to keep GitHub in sync:

```bash
cd /tmp && rm -rf simpladocs-sales && gh repo clone racheloarnold-bot/simpladocs-sales simpladocs-sales
cp ~/Desktop/simpladocs-day-plan.html /tmp/simpladocs-sales/day-planner/simpladocs-day-plan.html
cd /tmp/simpladocs-sales && git add . && git commit -m "Day planner update — $(date +%Y-%m-%d)" && git push
```

## Workflow

The full workflow has 7 steps, but the user may jump in at any point. Meet them where they are.

### Step 1: Gather context

Pull today's calendar events from Google Calendar. Check all relevant calendars (primary, work, prayer/salat, community calendars like Bay Area Meshk). Note fixed commitments, prayer times, and evening events that create hard deadlines.

### Step 2: Brain-dump

First prompt: *"Before we start — click the 'Copy capture notes' button in your day planner's Notes tab (top-right of the Capture section) and paste anything you want me to factor in."* Then ask what else is on their plate. They may provide a voice-transcribed dump, a typed list, or reference tasks from memory/past conversations. Capture everything without filtering.

### Step 3: Prioritize

If the user has many tasks, use the **prioritizer widget** (see `references/prioritizer-widget.md`). This is a two-column interactive tool where tasks can be moved between "Later" and "Today" with time estimates. The widget outputs a structured list via `sendPrompt()`.

If the user wants to skip the prioritizer (small number of tasks, or they already know what's important), move straight to scheduling.

### Step 4: Check calendar + constraints

Cross-reference tasks against calendar blocks. Identify:
- Hard commitments (meetings, prayer times, events)
- Prep tasks that need lead time (e.g., cooking before an event)
- Dependencies and blockers
- Best windows for deep work vs. admin

### Step 5: Propose schedule

Present the proposed day as a text plan first. Group tasks into time blocks around fixed events. Flag any conflicts or tight windows. Get user confirmation before building the widget.

### Step 6: Create calendar events (optional)

If the user wants, create Google Calendar events for the planned blocks. Only do this if explicitly requested — many users prefer the checklist alone.

### Step 7: Build the checklist

Build the **interactive checklist widget** (see `references/checklist-widget.md`). This is the core deliverable — a time-blocked, interactive day view with checkboxes, progress tracking, carry-over support, and the ability to add new tasks on the fly.

## Updating mid-day

The user will often come back mid-day with changes:
- New tasks discovered
- Tasks completed that need checking off
- Priorities shifted
- Blocks that need rearranging

When rebuilding the checklist, **always pre-seed the checked state** from:
- Tasks confirmed as done in conversation
- Screenshots showing completed items
- Prior widget state if discussed

Never make the user re-check things they've already completed.

## Key principles

- **Prioritize first, schedule second.** Don't create calendar events until the task list is settled.
- **Respect prayer times.** These are non-negotiable fixed points in the schedule.
- **Flag prep tasks early.** Things like cooking for an event need their time estimated and blocked before deep work gets scheduled.
- **Be honest about capacity.** If the day is overloaded, say so. Mark stretch goals explicitly.
- **Carry over gracefully.** Tasks that don't get done aren't failures — they carry over to tomorrow.
