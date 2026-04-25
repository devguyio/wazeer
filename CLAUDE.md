# CLAUDE.md

## What This Is

This repository is a Claude Code plugin marketplace containing the **`wz`** plugin — Wazeer, a personal GTD mentor, coach, and operations strategist.

The repo serves two roles:
1. **Marketplace** (`.claude-plugin/marketplace.json` at root) — lets users discover and install the plugin
2. **Plugin** (`plugins/wz/`) — the actual plugin with skills, references, and manifest

Users install the plugin, create an empty directory for their vault, and say "let's setup wazeer using the wazeer-setup skill". The setup skill generates all vault files interactively. The vault is the user's workspace — it lives on the user's machine, not in this repo.

## Repository Structure

```
.claude-plugin/
  marketplace.json              Marketplace catalog (name: "wazeer", one plugin: "wz")

plugins/wz/                     The wz plugin
  .claude-plugin/
    plugin.json                 Plugin manifest (name: "wz", version, author, license)
  skills/
    ping/SKILL.md               /wz:ping — vault reconciliation, status updates
    tidy/SKILL.md               /wz:tidy — vault folder hygiene
    wazeer-setup/                Setup skill (user-invocable: false)
      SKILL.md                  First-run: creates vault, configures profile, learns board format
      references/
        claude-md-template.md   Template for the vault's CLAUDE.md
        dump-skill.md           Source for /wz:dump — installed globally by setup
        board-format-example.md Structural reference for board-format.md generation
        status-template.md      Initial _wazeer/Status.md with empty frontmatter
        personas/
          veteran-cto.md        Default persona — veteran CTO mentor
    board-management/           Board operations skill (user-invocable: false)
      SKILL.md                  Card CRUD, slug IDs, tags, archiving rules
      references/
        rich-card-template.md   Template for rich card companion notes

docs/                           Human-facing documentation
  getting-started.md            Install and setup walkthrough
  concepts.md                   GTD flow, board structure, card conventions
  customization.md              Persona tweaks, faith-aware mode, custom tags

README.md                       Project overview and install instructions
```

## Plugin Architecture

### Namespacing

The plugin is named `wz` in `plugin.json`. Claude Code namespaces all skills under `wz:`, so:
- `ping` skill → `/wz:ping`
- `dump` skill → `/wz:dump` (standalone global skill, installed by setup to `~/.claude/skills/wz_dump/`)
- `tidy` skill → `/wz:tidy`

### Skill Visibility

**Plugin skills** (project scope, installed via `/plugin install wz@wazeer`):

| Skill | `user-invocable` | Behavior |
|-------|-----------------|----------|
| ping | `true` (default) | Appears in `/` menu, user invokes directly |
| tidy, wazeer-setup, board-management | `false` | Hidden from `/` menu, Claude auto-loads based on description match |

**Standalone skill** (user-wide, installed globally by setup to `~/.claude/skills/wz_dump/`):

| Skill | Behavior |
|-------|----------|
| dump (`wz:dump`) | Appears in `/` menu from any Claude Code session |

### Vault vs Plugin

The plugin is read-only once installed (cached at `~/.claude/plugins/cache/`). It cannot modify its own files at runtime. All runtime state lives in the user's vault:

| Concern | Where it lives | Written by |
|---------|---------------|------------|
| Board format rules | `_wazeer/board-format.md` (vault) | wazeer-setup skill during onboarding |
| Card slug definitions | `CLAUDE.md` Card Slugs table (vault) | wazeer-setup skill during onboarding |
| User profile | `CLAUDE.md` Your Profile section (vault) | wazeer-setup skill during onboarding |
| Boards, notes, inbox | vault root | ping, dump, board-management skills |
| Skill logic | `plugins/wz/skills/` (plugin) | developers in this repo |

### Board Format Agnosticism

The board-management skill does NOT hardcode markdown syntax. Instead:
1. During setup, the user installs their kanban plugin (Obsidian Kanban Plus, Neovim super-kanban, etc.)
2. They create a sample board, and the setup skill reads it to learn the format
3. Format rules are written to `_wazeer/board-format.md` in the vault
4. All board-touching skills (ping, board-management, tidy) read that file before modifying boards

Supported editors/plugins:
- **Obsidian**: Kanban Plus (recommended), original Kanban plugin
- **Neovim**: super-kanban.nvim, kanban.nvim
- **Other**: Plain markdown — boards work as text files with any editor

### Cross-References

All skill-to-skill references use the `/wz:` prefix. Never use bare `/ping` or `/dump` — always `/wz:ping`, `/wz:dump`, `/wz:tidy`.

The ping skill calls `/wz:tidy` during full reconciliation. The setup skill runs `/wz:ping fresh` at the end of onboarding.

## User Install Flow

```
mkdir ~/Documents/Obsidian/Wazeer && cd ~/Documents/Obsidian/Wazeer
claude
> /plugin marketplace add devguyio/wazeer
> /plugin install wz@wazeer --scope project
> "let's setup wazeer using the wazeer-setup skill"
```

The setup skill creates the vault structure, asks about editor/kanban plugin, learns the board format, collects profile info, creates boards, and runs a first ping.

## Development

### Testing Locally

From this repo:
```bash
claude --plugin-dir ./plugins/wz
```

Or add as a local marketplace:
```
/plugin marketplace add ./
/plugin install wz@wazeer --scope project
```

### Modifying Skills

Skills are in `plugins/wz/skills/<name>/SKILL.md`. Each skill has:
- YAML frontmatter with `name`, `description`, and optionally `user-invocable: false`
- Markdown body with instructions Claude follows when the skill is active

The `description` field determines when Claude auto-loads a skill. Include specific trigger phrases that users would say.

### Adding a New Skill

1. Create `plugins/wz/skills/<skill-name>/SKILL.md`
2. Add frontmatter with `name` and `description`
3. Set `user-invocable: false` if it should be auto-triggered only
4. If the skill touches boards, add instructions to read `_wazeer/board-format.md` first

### Updating the Vault Template

The vault's CLAUDE.md is generated from `plugins/wz/skills/wazeer-setup/references/claude-md-template.md`. Edit that file to change what new vaults get. Existing vaults are not affected — users would need to re-run setup or manually update their CLAUDE.md.

### Version Bumps

The plugin uses explicit versioning in `plugins/wz/.claude-plugin/plugin.json`. Bump the `version` field for users to receive updates. Without a version bump, cached copies are not refreshed.
