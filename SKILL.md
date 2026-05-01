---
name: session-summary
description: Auto-generate concise summaries at the end of each Hermes session capturing decisions made, tasks completed, todos created, and files modified — then save to Obsidian daily notes with wiki-links.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  tags:
    - productivity
    - session
    - summary
    - obsidian
    - notes
    - automation
  related_skills:
    - routine-optimizer
    - cron-visualizer
commands:
  - name: generate
    description: Generate a session summary
    args:
      - name: length
        type: string
        default: brief
        description: Summary length (brief, detailed, full)
      - name: session_id
        type: string
        default: current
        description: Session ID to summarise (default: current session)
      - name: save
        type: boolean
        default: true
        description: Whether to save to Obsidian daily note
      - name: clipboard
        type: boolean
        default: false
        description: Copy summary to clipboard
  - name: show
    description: Display session summary without saving
    args:
      - name: length
        type: string
        default: brief
        description: Summary length
      - name: session_id
        type: string
        default: current
        description: Session ID to show
  - name: config
    description: Configure session summary preferences
    args:
      - name: default_length
        type: string
        default: brief
        description: Default summary length for auto-generation
      - name: auto_generate
        type: boolean
        default: true
        description: Auto-generate summary at session end
      - name: include_sections
        type: string
        default: all
        description: Comma-separated sections to include (decisions,tasks,todos,files,links,all)
  - name: history
    description: View summaries from previous sessions
    args:
      - name: count
        type: integer
        default: 5
        description: Number of recent summaries to show
      - name: date
        type: string
        default: ""
        description: Specific date (YYYY-MM-DD) to view summaries from
---

# Session Summary

Never lose track of what you accomplished in a Hermes session. This skill automatically captures your decisions, completed tasks, new todos, and modified files, then saves a structured summary to your Obsidian daily note with proper wiki-links.

## How It Works

1. **Session Tracking**: Monitors the current Hermes session for activity — commands run, files read/written, decisions made, and tasks completed
2. **Event Capture**: Hooks into Hermes session lifecycle events to record key moments
3. **Summary Generation**: At session end (or on demand), compiles all tracked activity into a structured summary
4. **Obsidian Integration**: Saves the summary to your daily note with wiki-links to referenced files, projects, and people
5. **Configurable Depth**: Choose brief (key points), detailed (organized sections), or full (comprehensive log) output

## Commands Reference

### `generate`

Generate and optionally save a session summary.

```bash
# Quick brief summary, saved to daily note
hermes session-summary generate

# Detailed summary saved to daily note
hermes session-summary generate --length detailed

# Full comprehensive summary
hermes session-summary generate --length full

# Generate without saving to Obsidian
hermes session-summary generate --save false

# Generate and copy to clipboard
hermes session-summary generate --clipboard true

# Summarise a specific past session
hermes session-summary generate --session_id sess_20260501_073000
```

### `show`

Preview the summary without saving.

```bash
# Preview brief summary
hermes session-summary show

# Preview detailed summary
hermes session-summary show --length detailed

# Preview full summary for a specific session
hermes session-summary show --session_id sess_20260501_073000 --length full
```

### `config`

Configure session summary behavior.

```bash
# Set default to detailed summaries
hermes session-summary config --default_length detailed

# Disable auto-generation (manual only)
hermes session-summary config --auto_generate false

# Only include specific sections
hermes session-summary config --include_sections "decisions,tasks,todos"

# Re-enable all sections
hermes session-summary config --include_sections all
```

### `history`

Browse past session summaries.

```bash
# View 5 most recent summaries
hermes session-summary history

# View 10 most recent
hermes session-summary history --count 10

# View summaries from a specific date
hermes session-summary history --date 2026-04-30
```

## Summary Formats

### Brief (Default)

Compact summary focusing on the essentials:

```markdown
## Session Summary — 2026-05-01 07:30

**Duration**: 45min | **Tasks**: 4 completed, 1 pending | **Files**: 3 modified

### Key Decisions
- Adopted 06:00 wake time for A/B test starting Monday
- Switched to 4-meal spacing for protein distribution

### Completed
- ✅ Updated routine-optimizer config with weight (81.2kg)
- ✅ Analysed 7-day sleep patterns
- ✅ Set up morning brief cron job (07:00 daily)
- ✅ Imported apple health step data

### New Todos
- [ ] Evaluate wake time A/B test results on 2026-05-08
- [ ] Add evening stretch to habit tracker

### Files Modified
- `0-Daily/2026-05-01.md` — Added energy log and session summary
- `3-Resources/routine-reports/2026-W18.md` — Generated weekly report
- `~/.hermes/config.yaml` — Added routine-optimizer settings
```

### Detailed

Expanded with organized sections and wiki-links:

```markdown
## Session Summary — 2026-05-01 07:30–08:15

**Duration**: 45min | **Tasks**: 4/5 | **Files**: 3 | **Commands**: 12

### Context
- Working from [[Oakham]] home setup
- Continued from [[2026-05-01]] morning session
- Profile: 81.2kg, 1930 kcal target, home workouts

### Decisions Made
1. **Wake time A/B test** — Start 06:00 vs 07:00 wake time test Monday
   - Rationale: Morning workout data suggests +1.1 focus score
   - Related: [[routine-optimizer]] [[2026-W18-Experiment]]
2. **Meal spacing change** — Switch from 3 to 4 meals for better protein distribution
   - Rationale: 162g protein target difficult in 3 meals
   - Related: [[fitness-nutrition]] [[Meal Plan v3]]

### Tasks Completed
- ✅ Updated [[routine-optimizer]] config with current weight (81.2kg)
- ✅ Ran 7-day sleep pattern analysis
- ✅ Configured morning brief cron job (07:00 Europe/London)
- ✅ Imported Apple Health step data via [[apple-health-sync]]
- ⏳ Weekly report generation (pending — cron scheduled for Sunday)

### New Todos Created
- [ ] Evaluate wake time A/B test results on [[2026-05-08]] ⏳
- [ ] Add evening stretch routine to [[habits]] tracker
- [ ] Review [[Meal Plan v3]] macros after 3 days on 4-meal schedule

### Files Modified
| File | Change |
|------|--------|
| [[2026-05-01]] | Added energy logs, session summary, A/B test note |
| [[2026-W18-Report]] | Generated weekly routine optimization report |
| `~/.hermes/config.yaml` | Added routine-optimizer and cron settings |

### Links Referenced
[[routine-optimizer]] · [[fitness-nutrition]] · [[apple-health-sync]] · [[habits]] · [[2026-W18-Experiment]]
```

### Full

Complete log with every command, interaction, and detail:

Same structure as Detailed, plus:
- **Full Command Log**: Every command executed with timestamps
- **Raw Data**: Complete analysis results, not just summaries
- **Conversation Excerpts**: Key decision points from the conversation
- **Performance Metrics**: Command execution times and resource usage

## Configuration

Add to your Hermes config (`~/.hermes/config.yaml`):

```yaml
session-summary:
  # Where to save in Obsidian
  obsidian_vault: ~/obsidian-vault
  daily_note_path: "0-Daily"
  # Summary behavior
  default_length: brief
  auto_generate: true
  auto_generate_on_exit: true
  include_sections:
    - decisions
    - tasks
    - todos
    - files
    - links
  # Wiki-link style for Obsidian
  use_wikilinks: true
  # Tag for session summaries
  summary_tag: "session-summary"
  # Append to daily note or create separate file
  mode: append
  # Whether to include timestamps in brief mode
  include_timestamps: true
  # Timezone for timestamps
  timezone: Europe/London
```

## Obsidian Integration

### Daily Note Append Mode

By default, summaries are appended to your daily note under a `## Session Summaries` heading:

```markdown
# 2026-05-01

... existing daily note content ...

## Session Summaries

### 07:30–08:15 (Brief)
✅ 4 tasks · 💡 2 decisions · 📝 2 new todos
- Adopted 06:00 wake time A/B test
- Switched to 4-meal protein distribution
- [[2026-W18-Experiment]] · [[Meal Plan v3]]

### 14:00–14:20 (Brief)
✅ 2 tasks · 💡 1 decision · 📝 0 new todos
- Updated workout log with morning push-day results
- Reviewed afternoon energy patterns
- [[fitness-nutrition]] · [[Push Day Log]]
```

### Separate File Mode

Set `mode: separate` to create individual files:

```
~/obsidian-vault/0-Daily/2026-05-01/
├── session-0730.md
└── session-1400.md
```

Each file is named with the session start time and fully linked from the daily note.

### Wiki-Link Generation

The skill automatically generates Obsidian wiki-links for:
- Files modified during the session → `[[filename]]`
- Skills used → `[[skill-name]]`
- Dates referenced → `[[2026-05-08]]`
- Projects and concepts from your vault → detected from note content

## Session Tracking Internals

```python
# What gets tracked during a session
class SessionTracker:
    started_at: datetime          # Session start timestamp
    ended_at: datetime            # Session end timestamp
    commands: list[Command]       # All hermes commands executed
    files_read: list[str]         # Files read during session
    files_modified: list[str]     # Files written/created
    decisions: list[Decision]     # Explicit decisions captured
    tasks_completed: list[Task]   # Completed tasks
    todos_created: list[Todo]      # New todos/action items
    links_referenced: list[str]   # Notes, projects, skills referenced
    skills_used: list[str]        # Hermes skills invoked
```

Decision capture uses natural language detection — when you say "let's go with X" or "I'll choose Y", it's recorded as a decision.

## Pitfalls

1. **Incomplete session data**: If Hermes crashes or is force-quit, the session tracker may not finalize. The skill does a best-effort save on crash signals (SIGTERM, SIGINT) but SIGKILL is uncatchable.
2. **Large sessions**: Full-length summaries for very long sessions can be 500+ lines. Consider `brief` or `detailed` for routine sessions and reserve `full` for when you need an audit trail.
3. **Wiki-link false positives**: The link generator may create wiki-links to file names that don't exist in your vault. This is harmless in Obsidian (clicking creates the note) but may clutter your graph view. Set `use_wikilinks: false` if this bothers you.
4. **Daily note format assumptions**: Assumes PARA-based daily notes in `0-Daily/`. If your vault structure differs, update `daily_note_path` in config.
5. **Concurrent sessions**: Running two Hermes instances simultaneously creates two session trackers. Summaries are saved sequentially and should not conflict, but timestamps may overlap.
6. **Decision capture accuracy**: The NLP-based decision detection isn't perfect. It may miss implicit decisions or flag "let's think about X" as a decision. Review the Decisions section and correct as needed.
7. **Obsidian sync conflicts**: If you use Obsidian Sync or iCloud, there's a small risk of sync conflicts when the skill appends to daily notes that are being edited on another device. The skill uses atomic file appends to minimize this risk.

## Verification Steps

1. **Basic generation**: Run `hermes session-summary generate --save false` — should produce a summary to stdout, even if the session was short.
2. **Obsidian save**: Run `hermes session-summary generate` and check that the summary appears in today's daily note at `~/obsidian-vault/0-Daily/YYYY-MM-DD.md`.
3. **Wiki-links**: Open the daily note in Obsidian and verify that `[[links]]` are present and resolve to existing notes.
4. **Config changes**: Run `hermes session-summary config --default_length detailed` then `hermes session-summary show` — should display a detailed summary.
5. **History**: After completing at least two sessions, run `hermes session-summary history --count 3` — should list summaries from previous sessions.
6. **Brief vs Detailed vs Full**: Generate summaries at each length and verify the content depth increases appropriately.
7. **Clipboard**: Run `hermes session-summary generate --clipboard true --save false` then paste — should contain the summary.
8. **Crash recovery**: Kill a Hermes session mid-task (Ctrl+C), then start a new session and run `hermes session-summary history` — should show the interrupted session with partial data.