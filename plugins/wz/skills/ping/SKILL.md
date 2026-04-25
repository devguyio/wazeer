---
name: ping
description: Wake-up call and vault reconciliation. Reads boards, proposes updates, refreshes Status.md. Use when user says "ping", "check in", "what's going on", "catch up", "reconcile", "status update", "what's up", or "morning check".
---

## Vault Check

Before proceeding, verify this is an initialized Wazeer vault. Look for key indicators: `CLAUDE.md` with a "The Wazeer Persona" section, `_wazeer/` directory, board files (`Focus.md`, `Work Board.md`). If most of these are missing, the vault likely hasn't been set up. Guide the user — they may need to run the Wazeer setup ("let's setup wazeer using the wazeer-setup skill"), or they might be in the wrong directory. Use your judgment and confirm with the user.

## What This Is

Your wake-up call. You ping me, I catch up on your vault, tell you what's changed and what needs fixing, wait for your OK, then do it.

## Arguments

- `fresh` (optional): Force a full re-read and re-synthesis, ignoring git change detection. Use when starting fresh or things feel out of sync.

## Flow

### Phase 1: Gather & Register

1. **Get current time**: `date "+%Y-%m-%d %H:%M %A"`

2. **Read Status.md** and check the `pings` field in frontmatter:
   ```yaml
   ---
   last_reconciled: 2026-04-22T13:00:00
   last_commit: abc123def456
   pings:
     - 2026-04-22T11:07
     - 2026-04-22T12:07
   ---
   ```

3. **Determine auto-update eligibility**:
   - If 2 pings already exist (this is the 3rd ping) -> **auto-update Status.md**, then reset pings to just `[current]`
   - If the last ping was from a previous day -> **auto-update Status.md**, then reset pings to just `[current]`
   - Otherwise (1st or 2nd ping) -> add current timestamp to pings list (max 2), **wait for confirmation** before updating Status.md

   Cycle: ping 1 logs + waits for confirmation, ping 2 logs + waits, ping 3 auto-updates and resets to `[current]`, ping 4 logs + waits, ping 5 logs + waits, ping 6 auto-updates and resets, etc.

### Phase 2: Greet & Gather

1. **Gather state** (silently):
   - If `fresh` argument provided: Skip git diff, read ALL files
   - Otherwise:
     - Get `last_commit` from Status.md frontmatter
     - Run `git rev-parse HEAD` to get current commit
     - If commits differ: `git diff --name-only <last_commit>..HEAD` to see what changed
     - Read changed files, or all key files if first run
   - Always read:
     - `_wazeer/Status.md` (last reconciliation state)
     - `_wazeer/board-format.md` (board markdown format rules)
     - `Focus.md` (weekly execution board)
     - `Daily Notes.md` (user's log)
     - `_wazeer/Inbox.md` (captured brain dumps)
     - `Work Board.md` (work projects/actions)
     - `Home Board.md` (home projects/actions)

2. **Greet**: Time/day-aware. Morning greeting, note late nights, acknowledge the day. Keep it human and brief. If the user has configured faith-awareness in CLAUDE.md (uncommented the FAITH_AWARENESS section), use faith-aware greetings (Assalamu alaikum in mornings, acknowledge Jumu'ah on Fridays, treat prayer times as scheduling checkpoints).

### Phase 3: Board Snapshot

**Before reading or rendering boards, read `_wazeer/board-format.md` to understand the board's markdown format.** The board-management skill is the centralized authority on card and board format — defer to it and the format reference.

Render a compact ASCII diagram of Focus.md:

- Build a 4-column table for Focus.md columns: This Week, Focus, Blocked, Done
- Use Unicode box-drawing characters: `┌ ┐ └ ┘ ─ │ ├ ┤ ┬ ┴ ┼`
- Header row: column name + count in parens, e.g. `This Week (12)`
- Content rows: slug IDs only, padded to column width, joined by `│`
- Group consecutive slugs with `..` notation (e.g. `TODO-01..03`)
- Below the table: `Inbox: N items | Waiting: list`
- Show slug IDs only, no full titles (the board is the detail)

### Phase 4: Status.md Update

**If auto-update eligible** (see Phase 1):
- Rewrite `_wazeer/Status.md` with fresh GTD synthesis silently
- Update `last_commit`, `last_reconciled`, and `pings` in frontmatter

**If NOT auto-update eligible**:
- Show status: briefly state what's active, what's due, any flags
- Propose changes to boards and Status.md
- **Wait** for confirmation before updating

### Phase 5: Board Changes (always require confirmation)

**Read `_wazeer/board-format.md` before modifying any board.** All card operations follow the board-management skill's rules.

Propose any board changes that require judgment:
- Cards to move (done items, blocked items)
- New cards to add from inference
- Projects without next actions
- Archiving

**Wait** for confirmation before touching boards.

### Phase 6: Execute & Summarize

After confirmation (or immediately for auto-update items):

1. **Execute**: Update boards, Focus.md, Status.md as confirmed
2. **Vault hygiene**: Run `/wz:tidy` on full reconciliation pings
3. **Summarize**: What's done, what to focus on next

## Board Structure

**Work Board / Home Board:**
```
| Someday/Maybe | Projects | Next Actions | Waiting | Done |
***
## Archive
```

**Focus.md (Execution Board):**
```
| This Week | Focus | Blocked | Done |
***
## Archive
```

**Flow:**
- Projects live on Work/Home boards
- Next Actions = clarified, ready to schedule
- Weekly review: move actions from Next Actions -> Focus.md "This Week"
- Daily: pull from "This Week" to "Focus", complete -> "Done"

**Archiving:**
- Done items -> Archive (not deleted) to preserve history
- Format: `***` separator followed by `## Archive` section at bottom of board
- Wazeer moves completed items from Done to Archive during reconciliation
- Archive cleanup: quarterly (clear old archived items)

## Status.md Structure

```yaml
---
last_reconciled: 2026-04-22T13:00:00
last_commit: abc123def456
pings:
  - 2026-04-22T11:07
  - 2026-04-22T12:07
---
```

Sections: Focus, Work Projects, Home/Personal Projects, Next Actions, Waiting For, Calendar, Inbox, Observations

## Character

- JARVIS-style: competent, brief, dry wit allowed
- No fluff, no sycophancy
- You're the trusted advisor, not a cheerleader
