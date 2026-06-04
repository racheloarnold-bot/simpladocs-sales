---
name: day-planner
description: "Interactive daily planning workflow: brain-dump → prioritize → schedule → calendar → checklist. Use this skill whenever the user wants to plan their day, organize tasks, do a morning planning session, figure out what to work on, or says things like \"let's plan my day,\" \"what's on my plate,\" \"morning planning,\" \"help me organize today,\" \"what should I do today,\" \"daily standup,\" \"let's get organized,\" or references a brain-dump of tasks they need to sort through. Also trigger when the user asks to update, modify, or rebuild their day plan or checklist, or when they share a screenshot of their existing checklist and want it refreshed. Do NOT use for long-term project planning, weekly/monthly reviews, or goal-setting that isn't about today's specific schedule."
---

# Day Planner

An interactive daily planning workflow that produces a time-blocked HTML checklist, deployed to Vercel for stable URL access with persistent localStorage state.

## HTML Template Source

The canonical template lives in the cloned repo at:

```
/home/user/simpladocs-sales/day-planner/simpladocs-day-plan.html
```

Copy it to /tmp before modifying (all edits use Python binary mode):

```bash
cp /home/user/simpladocs-sales/day-planner/simpladocs-day-plan.html /tmp/simpladocs-day-plan.html
```

The template already contains: auto-update banner, carryover feature, Q2 Rocks tab with Coda seeding, Notes tab, water tracker, cherry blossom animation, and all CSS. Do NOT regenerate from scratch — always start from this file and only update the date-specific sections below.

## Daily customization (4 things to update)

Make all changes via Python in binary mode (`rb`/`wb`). Never use text mode — it corrupts encoding.

### 1. BLOCKS array

Replace the entire `const BLOCKS = [` array through (but not including) `function getOrderedBlocks()`. Use this pattern:

```python
blocks_start = html.find(b'const BLOCKS = [')
fn_start = html.find(b'function getOrderedBlocks()')
html = html[:blocks_start] + new_blocks + html[fn_start:]
```

Leave 2 blank lines between the closing `]` and `function getOrderedBlocks()`.

### 2. localStorage today key

Update the `today` key in the `const KEYS = {` object to today's date:

```python
html = html.replace(b"today: 'sd-today-jun02-v1'", b"today: 'sd-today-jun03-v1'")
```

Format: `sd-today-[3-letter-month][day]-v1` — e.g. `sd-today-jun03-v1`, `sd-today-jul4-v1`.
This ensures each day's checked state is separate.

### 3. BUILD_DATE

Update the JS constant used by the auto-update banner:

```python
html = html.replace(b"const BUILD_DATE = '2026-06-02'", b"const BUILD_DATE = '2026-06-03'")
```

Format: `YYYY-MM-DD`.

### 4. Header date display

Update the hardcoded date string in the header:

```python
html = html.replace(b'>Wednesday, June 3<', b'>Thursday, June 4<')
```

Find the current value by searching for `id="header-date"`.

## BLOCKS array format

```javascript
const BLOCKS = [
  { id:'prep', title:'Prep for Day', start:9.0, end:9.5, tasks:[
    { id:'t-meds', meds:true, text:'Meds', note:'Take with water' },
    { id:'t-prep-cal', text:'Review calendar: [meetings today]', note:'[context]' },
    { id:'t-prep-slack', text:'Clear Slack + email', note:'[anything urgent in Slack]' },
  ]},
  { id:'block-id', title:'Block Title', start:9.5, end:11.0, tasks:[
    { id:'t-taskid', hi:true, text:'High priority task', note:'Context note' },
    { id:'t-taskid2', med:true, text:'Medium priority task', note:'Context note' },
    { id:'t-taskid3', text:'Normal task', note:'Context note' },
  ]},
  { id:'lunch', title:'Lunch', start:12.0, end:13.0, tasks:[], isBreak:true },
  { id:'wrapup', title:'Daily Wrap Up', start:16.0, end:17.0, tasks:[
    { id:'t-wu-hs', med:true, text:'HubSpot EOD: update all touched deals with next steps', note:'Every deal you touched today needs a next step logged before close.' },
    { id:'t-wu-capture', text:'Update capture notes for tomorrow', note:'Log blockers, incomplete items, anything that surfaced today.' },
  ]},
  { id:'gym', title:'Gym', start:18.5, end:20.0, tasks:[], isBreak:true },
]
```

**Task flags:**
- `hi:true` — hot pink highlight with fire emoji, use for top-priority items (max 4-6 per day)
- `med:true` — purple highlight, use for important but not urgent items
- `meds:true` — lime green, only for the meds reminder task
- `isBreak:true` — renders as a break card with no task list

**Scheduling rules:**
- Day always starts at 9:00 AM
- First block: "Prep for Day" 9:00-9:30
- Last work block: "Daily Wrap Up" 4:00-5:00 PM
- Gym/evening block at 6:30 PM with `isBreak:true`
- Max 4-5 tasks per block
- Max 4-6 `hi:true` tasks across the entire day
- Always include `meds:true` task in Prep block

**Task IDs:** Use short, stable, descriptive IDs (e.g. `t-az-docusign`, `t-spring-sig`). These persist in localStorage so keep them consistent if a task carries over across days.

## What NOT to update

- **HubSpot Tasks tab**: removed from the planner entirely — do not add it back
- **Q2 Rocks tab**: automatically seeded from Coda state on first load. Only update the seed list in `seedRocksIfEmpty()` when new rocks items are completed in the Coda Q2 Rocks doc.
- **Carryover feature**: fully automatic. If Rachel clicked "Save for tomorrow" the previous day, a "From yesterday" banner will appear automatically on the next load.
- **Auto-update banner JS**: already in template, no changes needed beyond updating `BUILD_DATE`.
- **CSS / JS infrastructure**: all features are baked in. Never regenerate these sections.

## Data sources for BLOCKS content

Pull from these sources to populate today's BLOCKS:

1. **Google Calendar** (`mcp__Google-Calendar__list_events`): today's meetings, fixed commitments, hard time constraints
2. **Granola** (`mcp__Granola__get_meetings` or `list_meetings`): action items from recent meetings Rachel attended
3. **HubSpot** (`mcp__HubSpot__get_crm_objects` or `query_crm_data`): open deals with overdue close dates, deals needing follow-up, tasks due today
4. **Slack** (`mcp__Slack__slack_read_channel`): anything urgent or time-sensitive that came in
5. **capture-notes.md** (read from repo at `day-planner/capture-notes.md`): carryover items and context from previous days
6. **rachel-brain.md** (read from repo): Rachel's context, preferences, current focus areas

## Pushing to GitHub / Vercel

After building, copy back to the repo and push:

```bash
cp /tmp/simpladocs-day-plan.html /home/user/simpladocs-sales/day-planner/simpladocs-day-plan.html
cd /home/user/simpladocs-sales
git add day-planner/simpladocs-day-plan.html
git commit -m "Day planner — YYYY-MM-DD"
git push -u origin main
```

Vercel auto-deploys within ~30 seconds of the push. Rachel's bookmarked URL updates automatically — no download needed.

If push is rejected (another process pushed first), run `git fetch origin main && git rebase origin/main` then push again.

## Capture notes bridge

The Notes tab stores capture notes in the browser's localStorage — it cannot be read server-side. The `day-planner/capture-notes.md` file in this repo is the bridge between sessions.

**How to keep it in sync:**
- At the end of each planning session, write any outstanding capture items (tasks not yet in the plan, things to remember tomorrow) to `capture-notes.md` and commit it.
- When building the next day's plan, read `capture-notes.md` as a data source alongside Calendar, HubSpot, Granola, and Slack.
- If running interactively, also prompt: *"Anything else on your plate I should factor in?"* — treat the answer as additional brain-dump input.

**Important:** `capture-notes.md` is the source of truth for the automated morning run. If it's stale, capture notes won't be reflected. Keep it updated.

## Workflow

The full flow has 5 steps. Meet Rachel where she is — she may jump in mid-flow.

### Step 1: Gather context
Pull Google Calendar, Granola action items, HubSpot open tasks and deal status, Slack urgents, and capture-notes.md. Read rachel-brain.md for current context.

### Step 2: Brain-dump (optional)
If running interactively: prompt for the capture notes paste, then ask what else is on her plate.

### Step 3: Prioritize
Identify the 4-6 most critical items (these get `hi:true`). Everything else slots in by energy/time required.

### Step 4: Build BLOCKS
Lay out the day time-block by time-block starting at 9 AM. Anchor to fixed calendar events. No more than 4-5 tasks per block.

### Step 5: Build and push
Apply the 4 daily customizations (BLOCKS, today key, BUILD_DATE, header date), push to GitHub, confirm push succeeded.

## Key principles

- **Day starts at 9 AM.** Starting earlier makes Rachel feel behind before she starts. If she starts early, being ahead is a bonus.
- **Max 4-6 hi:true tasks.** More than that and nothing is actually high priority.
- **Max 4-5 tasks per block.** Blocks with 6+ tasks are stressful, not helpful.
- **Never re-check completed tasks.** If a task is known done, pre-seed it or leave it off.
- **Carry over gracefully.** Unchecked tasks from yesterday surface automatically via the carryover banner if Rachel used "Save for tomorrow."
- **Deal context in notes.** Every deal task should have a `note` field with enough context to act without opening HubSpot.
- **Every external client call gets a follow-up block.** The block immediately after any external client call must always include three tasks: (1) send the follow-up email, (2) log next steps in HubSpot, (3) confirm call notes are saved in HubSpot. No exceptions.
- **Deal scan is a single daily habit, not a task list.** One task in the Prep block: "Deal scan: review HubSpot, confirm next steps are current." Do not break this out deal-by-deal unless a specific deal has a genuine action item due today.
- **Don't create tasks for things already done.** Check HubSpot next steps before adding any deal-related task. If it's already logged and actioned, leave it off.
- **Pipeline review blocks don't need task lists.** If there's a weekly pipeline review on the calendar, don't populate it with deal-by-deal tasks — that's what the meeting is for. Only flag a deal in that block if there's something Rachel specifically needs to raise or decide.
