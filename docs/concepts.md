# Concepts

How the Wazeer GTD system works under the hood.

## GTD in 30 Seconds

Getting Things Done (GTD) is David Allen's productivity methodology. The core idea: get everything out of your head and into a trusted system, then work that system. Wazeer is that system.

The flow:
1. **Capture** - dump anything into the inbox, don't think about it
2. **Clarify** - process inbox items: is it actionable? what's the next step?
3. **Organize** - put it on the right board in the right column
4. **Review** - weekly review to keep the system current
5. **Engage** - pick from your Focus board and do the work

## The Three Boards

Wazeer uses three Kanban boards, each with a distinct purpose. The boards work with any editor — Obsidian, Neovim, or plain text. The exact markdown format is learned during setup and stored in `_wazeer/board-format.md`.

### Focus.md (Execution)

What you're doing **this week**. This is your daily cockpit.

| Column | Purpose |
|--------|---------|
| **This Week** | Items committed for this week, pulled from board Next Actions |
| **Focus** | What you're working on right now (1-3 items max) |
| **Blocked** | Can't proceed - waiting on someone/something |
| **Done** | Completed this week |

### Work Board.md (Planning - Work)

Your work project backlog. Clarified but not scheduled.

| Column | Purpose |
|--------|---------|
| **Someday/Maybe** | Ideas and possibilities - not committed |
| **Projects** | Multi-step outcomes with defined next actions |
| **Next Actions** | Single concrete actions, ready to be pulled into Focus |
| **Waiting** | Delegated or blocked on external input |
| **Done** | Completed (archived periodically) |

### Home Board.md (Planning - Personal)

Same structure as Work Board, for personal life.

## Card Types and Slug IDs

Every card gets a slug ID prefix. This keeps things scannable and lets Wazeer reference cards concisely. Your slug categories are defined in the "Card Slugs" table in `CLAUDE.md` - that is the single source of truth. The examples below use TODO/REV/BUG as illustrative defaults, but you define your own categories during setup.

### Simple Cards

General tasks and action items. One-liners that are self-explanatory.

### Cards with Links

A card with a slug ID and title on the first line, and a link (thread, PR, ticket) on an indented second line.

### Complex Cards

A card that links to a companion note in `Projects/`. The note holds the detail; the card stays short.

### Rich Cards

Items that need structured, ongoing tracking. These are heavier cards with companion notes. Use for bugs, escalations, incidents, epics - anything that accumulates updates over time.

Rich cards always get a companion note at `Projects/<SLUG>/<SLUG>-XX Title.md` with Summary, Updates, and Next sections.

**When to use rich cards:** Any time something needs investigation and ongoing tracking. Common scenarios:
- Bug reports needing debugging
- On-call/pager duty incidents
- Escalations from other teams
- Production issues reported via chat
- Epics with multiple sub-tasks

The exact markdown syntax for all card types depends on your kanban plugin. Wazeer learns the format during setup and handles it for you.

## Tags

Tags make cards filterable and scannable. Every card gets at least one.

- **Type tags**: `#interrupt`, `#catch-up`
- **Domain tags**: whatever fits your work (e.g. `#backend`, `#infra`, `#frontend`, `#mobile`, `#data`)
- **Context tags**: `#work`, `#home`, `#medical`, `#finance`
- **Status tags**: `#urgent`, `#in-progress`, `#blocked-on-requester`

Define your own domain tags based on your projects and areas of work.

## The Weekly Review

The most important ritual. Do this Sunday or Monday morning.

1. **Process the inbox** - ask Wazeer to go through the inbox with you, clarify each item, move to boards
2. **Review Work Board and Home Board** - are projects current? Do they have next actions?
3. **Clear Done columns** - archive completed items
4. **Build the week** - pull items from Next Actions to Focus.md "This Week"
5. **Run `/wz:ping`** - let Wazeer reconcile and update Status.md

## Archiving

Completed items are never deleted. They move from Done to Archive. Each board has an Archive section. Wazeer handles the format based on your kanban plugin.

Wazeer moves items from Done to Archive during `/wz:ping` reconciliation. Archives are cleaned quarterly.

## Status.md

Status.md is Wazeer's internal state — a synthesized view of your entire GTD system. Wazeer writes and reads it during pings to track what's going on. It includes:

- Current focus and blockers
- Work and personal project tables
- Next actions grouped by context (@work, @home, @calls)
- Waiting-for list with who/what/since-when
- Inbox count
- Observations - patterns Wazeer notices (sliding items, late nights, overload)

Updated automatically during `/wz:ping`.

## The Ping Protocol

`/wz:ping` is the heartbeat of the system. It has a smart update mechanism:

- **1st-2nd ping of a session**: Wazeer proposes changes, waits for confirmation
- **3rd+ ping (or resuming from a previous day)**: Wazeer auto-updates Status.md, still asks before touching boards

This means early pings are collaborative (you approve changes), but once you're in flow, Wazeer stays out of your way.

## Immediate Updates vs. Proposals

There are two types of vault changes:

**Immediate** (no confirmation needed): When you tell Wazeer something is done, provide interrupt updates, or paste information. User-provided facts are instructions, not proposals.

**Proposed** (confirmation required): When Wazeer infers changes - suggesting cards to move, new items to add, things to archive. These require your OK first.

## Directory Structure

```
vault-root/
  Focus.md                  Weekly execution board
  Work Board.md             Work backlog
  Home Board.md             Personal backlog
  Daily Notes.md            Your daily log
  CLAUDE.md                 Wazeer configuration

  Projects/                 Active work
    TODO/                   Complex task companion notes
    BUG/                    Bug tracking companion notes
    YourProject/            Project-specific files

  Backlog/                  Parked ideas
  _archive/                 Completed projects
  _personal/                Personal documents
  _wazeer/                   System files
    Inbox.md                Brain dump inbox
    Status.md               Wazeer's GTD synthesis
    board-format.md         Board markdown format reference
```

Three lifecycle states: `Projects/` (active), `Backlog/` (parked), `_archive/` (done). Every project is a named subfolder - never loose files. Subfolders under `Projects/` match your slug prefixes and are auto-created when Wazeer makes companion notes.

