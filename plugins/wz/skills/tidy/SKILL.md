---
name: tidy
description: Scan vault for stray files and propose cleanup. Enforces folder structure and catches misplaced items. Used internally by the ping skill during full reconciliation.
user-invocable: false
---

## Vault Check

Before proceeding, verify this is an initialized Wazeer vault. Look for key indicators: `CLAUDE.md` with a "The Wazeer Persona" section, `_wazeer/` directory, board files (`Focus.md`, `Work Board.md`). If most of these are missing, the vault likely hasn't been set up. Guide the user — they may need to run the Wazeer setup ("let's setup wazeer using the wazeer-setup skill"), or they might be in the wrong directory. Use your judgment and confirm with the user.

## What This Is

Vault hygiene. Enforce the vault's folder structure, catch strays, and keep things visually clean. Visual clutter is cognitive load. If you can't tell what something is and whether it matters by glancing at the folder pane, it's wrong.

## Vault Structure

The vault follows a lifecycle-based folder hierarchy:

```
Root/
  Focus.md                - weekly execution board
  Work Board.md           - work projects backlog
  Home Board.md           - personal projects backlog
  Daily Notes.md          - daily log
  CLAUDE.md               - system config

  Projects/               - active work, one subfolder per project
    ProjectA/
    ProjectB/
    ...

  Backlog/                - parked ideas, come back later
    IdeaA/
    IdeaB/
    ...

  _personal/              - resume, CV, personal docs

  _archive/               - entire projects/topics that are done or superseded
    OldProjectA/
    ...

  _wazeer/                 - system (Status.md, Inbox.md, board-format.md)
  .obsidian/              - Obsidian config (if using Obsidian)
```

### Key Rules

1. **Three lifecycle states**: `Projects/` (active), `Backlog/` (parked), `_archive/` (done)
2. **Every project is a named subfolder** - never loose files directly in `Projects/`, `Backlog/`, or `_archive/`
3. **Project-level archives**: Active projects can have their own `_archive/` subfolder for completed artifacts within that project
4. **Archives are organized too**: Inside any `_archive/`, group files into named subfolders by topic. Never dump loose files into an archive - it becomes a graveyard
5. **Underscore prefix** (`_archive/`, `_personal/`, `_wazeer/`) sorts system/inactive folders to the bottom in Obsidian

### Examples

Active project with its own archived artifacts:
```
Projects/ProjectA/
  design.md
  notes.md
  _archive/
    Sprint 1/
      Sprint 1 - Research.md
      Sprint 1 - POC Comparison.md
```

Entire topic archived:
```
_archive/
  OldProjectA/
    planning.md
    notes.md
```

## Flow

1. **Scan** the vault root and all top-level content folders.

2. **Classify** each item:
   - **Core** (belongs at root): Focus.md, Work Board.md, Home Board.md, Daily Notes.md, CLAUDE.md
   - **System** (leave alone): `.obsidian/`, `.git/`, `_wazeer/`, `_personal/`, `_archive/`
   - **Lifecycle folders** (check contents): `Projects/`, `Backlog/`
   - **Stray**: anything else at root - files or folders that don't fit the structure

3. **Check structure violations**:
   - Loose files in `Projects/`, `Backlog/`, or `_archive/` (should be in named subfolders)
   - Loose files in any `_archive/` subfolder (should be grouped by topic)
   - Files at root that belong in a lifecycle folder
   - Empty files (propose deletion)
   - Duplicate files across locations

4. **Propose** actions for each issue found:
   - Move to the correct lifecycle folder and subfolder
   - Group loose archive files into a named subfolder
   - Delete if empty or clearly throwaway
   - Ask if unclear - don't guess lifecycle state

5. **Wait** for confirmation before executing.

6. **Execute** moves/deletes on confirmation.

## Rules

- Never touch core boards, system folders, or CLAUDE.md
- Never delete files with content without explicit confirmation
- Empty files can be proposed for deletion
- Prefer moving to existing folders over creating new ones
- When unsure whether something is Projects vs Backlog vs _archive, ask
- The boards (Focus.md, Work Board.md, Home Board.md) are the source of truth for what's active - folders tell you where things live
