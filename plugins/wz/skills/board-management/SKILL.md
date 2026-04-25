---
name: board-management
description: Manages Kanban board cards in personal knowledge management tools (Obsidian, Neovim, or any markdown editor) with consistent formatting, slug IDs, and tags. Use when creating, updating, moving, or archiving cards on Focus.md, Work Board.md, or Home Board.md. Triggers on "add card", "move to done", "track this", "add to board", or any board modification.
user-invocable: false
---

# Board Management

Manages all card operations on the vault's Kanban boards. Cards must be lean, scannable, and consistent.

**Before creating or modifying any board card, read `_wazeer/board-format.md` to understand the current board format. All card syntax, column structure, and whitespace rules come from that file.**

## Slug Configuration

Card slug prefixes are defined in CLAUDE.md under "Card Slugs". Read that table to know the current categories before creating cards.

## Instructions

### Step 1: Read board format

Read `_wazeer/board-format.md` to understand:
- How columns are defined in this vault's board files
- How cards are formatted (list items, headings, indentation)
- Whitespace rules (blank lines between cards, between columns)
- Any plugin-specific metadata blocks

All examples below use generic markdown. **Adapt them to match the format in `_wazeer/board-format.md`.**

### Step 2: Determine card type and board

Look up the slug categories defined in the Card Slugs table in CLAUDE.md. Pick the prefix that matches the card's type. Find the next available number for that prefix on the target board.

### Step 3: Create the card in the correct format

**Simple card (one-liner):**
```markdown
- TODO-01: Short title #context-tag #domain-tag
```

**Card with a link (second line):**
```markdown
- REV-01: Short title #review #domain-tag
    [thread](url)
```

**Complex card (with linked note):**
```markdown
- TODO-09: [[Projects/TODO/Title|Title]] #work #domain-tag
    [Ticket](url)
```

For complex cards, create a companion note in a matching subfolder under `Projects/` (e.g. `Projects/TODO/Title.md`).

**Rich card (for items needing structured tracking):**
```markdown
- #### [[Projects/BUG/BUG-XX Title|BUG-XX: Short title]]
    #bug #priority #status-tag
    **From** @person

    **Next**
    What's the next action or what we're waiting on
```

Rich cards use a heading for visual weight and get a companion note. Use this format for anything that needs ongoing tracking (bugs, escalations, incidents, epics). Consult `${CLAUDE_PLUGIN_ROOT}/skills/board-management/references/rich-card-template.md` for the note structure.

**Note on linking syntax:** The examples above use wikilinks (`[[path|display]]`). If your editor uses a different linking convention, check `_wazeer/board-format.md` for the syntax that works with your kanban plugin.

### Step 4: Apply tags

Every card gets at least one tag. Common tags:

- **Domain**: adapt to your domains (e.g. `#backend`, `#infra`, `#frontend`, `#mobile`)
- **Context**: `#work`, `#home`, `#medical`
- **Status**: `#blocked`, `#urgent`, `#in-progress`, `#waiting`

### Step 5: Place on the correct board and column

**Focus.md** columns: `This Week` | `Focus` | `Blocked` | `Done`
**Work Board.md / Home Board.md** columns: `Someday/Maybe` | `Projects` | `Next Actions` | `Waiting` | `Done`

- Rich/tracked cards go in the `Focus` column of Focus.md
- Simple cards go in `This Week` on Focus.md
- Backlog items go in `Next Actions` on Work/Home boards

## Card operations

### Moving to Done
Move the card from its current column to the Done column.

### Archiving
Move from Done to Archive (below the archive separator). Never delete - archive preserves history.

### Updating
- **Simple cards**: Edit the card directly
- **Rich cards**: Update only the **Next** line and tags on the card. Put detail updates in the companion note.

## Rules

1. **No walls of text** - if it doesn't fit in a glance, it goes in a linked note
2. **Every card gets a slug ID and tags** - no exceptions
3. **First line is always short** - title + tags only
4. **Links go on indented second line** - never inline with the title
5. **Use display aliases for linked cards** - e.g. `[[path|display name]]` for wikilinks. Check `_wazeer/board-format.md` for your editor's linking syntax
6. **Archive, don't delete** - completed items go to Archive section
7. **Follow whitespace rules from `_wazeer/board-format.md`** - some plugins break rendering on blank lines between cards, others require them
8. **Slug prefixes come from CLAUDE.md** - always read the Card Slugs table before assigning

## Examples

### Adding a rich card
User says: "new bug from @jsmith, database connection pool exhausted"

Result on Focus.md:
```markdown
- #### [[Projects/BUG/BUG-02 DB pool exhausted|BUG-02: DB pool exhausted]]
    #bug #urgent #backend #in-progress
    **From** @jsmith

    **Next**
    Investigate connection pool metrics and recent deployments
```
Plus companion note created at `Projects/BUG/BUG-02 DB pool exhausted.md`.

### Simple cards
```markdown
## This Week

- TODO-01: Draft design doc #work #backend
- TODO-02: Review PR for auth refactor #work #auth
- TODO-03: Update monitoring dashboards #work #infra
- REV-01: Team lead's architecture proposal #review
    [thread](https://chat-link)
```

### Completing a task
User says: "TODO-03 is done"

Move `- TODO-03: Update monitoring dashboards #work #infra` from its current column to the Done column.

## Common issues

### Card renders incorrectly in kanban view
The card has too much content or wrong formatting. Check `_wazeer/board-format.md` for the expected format. Move details to a linked note.

### Cards appear in wrong columns
Check whitespace rules in `_wazeer/board-format.md`. Some plugins use blank lines as list breaks.

### Linked card shows full path instead of title
Check `_wazeer/board-format.md` for the correct linking syntax with display aliases.

### Note paths
Companion notes go in `Projects/<SLUG>/` subfolders matching the card's prefix. Not all cards need notes - only complex ones with links, context, or multi-step tracking.

### Slug collision
Always read the current board state before assigning a slug number. Count existing slugs of that type and increment.
