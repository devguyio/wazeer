# Board Format Reference

This is an example of a completed `_wazeer/board-format.md` for Obsidian with the Kanban Plus plugin. The actual file in the user's vault should be generated from reading their sample board, not copied verbatim.

---

Editor: Obsidian
Plugin: Kanban Plus (fork of obsidian-kanban)

## Column Format

Columns are defined as `## Heading` sections. Each column is a level-2 markdown heading followed by list items (cards).

```markdown
## This Week

- TODO-01: Draft design doc #work #backend
- TODO-02: Review PR #work #auth
```

## Card Format

Cards are markdown list items (`- `). All content for a card must be contiguous — no blank lines between cards within the same column.

**Simple card:**
```markdown
- TODO-01: Short title #context-tag #domain-tag
```

**Card with link:**
```markdown
- REV-01: Short title #review #domain-tag
    [thread](url)
```

**Complex card (with companion note):**
```markdown
- TODO-09: [[Projects/TODO/Title|Title]] #work #domain-tag
    [Ticket](url)
```

**Rich card (structured tracking):**
```markdown
- #### [[Projects/BUG/BUG-XX Title|BUG-XX: Short title]]
    #bug #priority #status-tag
    **From** @person

    **Next**
    Current next action or blocker
```

Notes:
- Rich cards use `####` for visual weight in Kanban view
- Wikilinks use display aliases: `[[path|display name]]`
- The `**Next**` line is separated from tags by a blank line within the card, but there must be NO blank line between separate cards

## Metadata

The Obsidian Kanban plugin adds a settings block at the end of each board file:

```markdown
%% kanban:settings
```json
{"kanban-plugin":"basic"}
```
%%
```

Do not manually edit this block. The plugin manages it.

## Whitespace Rules

- **No blank lines between cards** in the same column. Blank lines between list items cause the Kanban plugin to split cards into separate columns.
- **One blank line after column heading** before the first card.
- **Archive separator**: `***` on its own line, followed by `## Archive`.

## Archive Format

```markdown
## Done

- TODO-05: Completed task #work

***

## Archive

- TODO-03: Previously completed task #work
```

## Example Board Snippet

```markdown
---

kanban-plugin: basic

---

## This Week

- TODO-01: Draft design doc #work #backend
- TODO-02: Review PR for auth refactor #work #auth
- REV-01: Team lead's architecture proposal #review
    [thread](https://chat-link)

## Focus

- #### [[Projects/BUG/BUG-03 Auth timeout|BUG-03: Auth timeout]]
    #bug #urgent #auth #in-progress
    **From** @ops-team

    **Next**
    Checking upstream dependency health.

## Blocked

## Done

***

## Archive

%% kanban:settings
```json
{"kanban-plugin":"basic"}
```
%%
```
