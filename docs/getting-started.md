# Getting Started

Set up your personal Wazeer GTD system in about 5 minutes.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview) (Anthropic's CLI for Claude)
- A text editor — [Obsidian](https://obsidian.md/) recommended, Neovim and others supported
- Git (for vault versioning and change tracking)

## Step 1: Create Your Vault and Install

Create a directory for your vault, start Claude Code, install the plugin, and run setup — all in one session:

```bash
mkdir ~/Documents/Obsidian/Wazeer && cd ~/Documents/Obsidian/Wazeer
claude
```

```
/plugin marketplace add devguyio/wazeer
/plugin install wz@wazeer --scope project
let's setup wazeer using the wazeer-setup skill
```

The plugin is installed at project scope, making `/wz:ping` available inside the vault. During setup, `/wz:dump` is installed globally so it works from any Claude Code session.

Wazeer walks you through everything interactively:
1. **Welcome** — what Wazeer is and how it works
2. **Vault structure** — creates directories, CLAUDE.md, system files, initializes git
3. **Editor & kanban** — picks your editor, installs a kanban plugin, learns the board format
4. **Version control** — how you want to handle commits
5. **Profile** — your name, role, strengths, challenges, what you need from Wazeer
6. **Boards** — slug prefixes, column layouts, tags
7. **Shell alias** — optional `wz` one-command startup
8. **First ping** — takes the system for a spin
9. **Summary** — recap of what was configured
10. **Verification** — adversarial review of the setup

### Or: Manual Setup

If you prefer to set things up yourself:

1. Create the vault directory structure manually
2. Copy and fill in a `CLAUDE.md` (see the template in the plugin's references)
3. Create your board files manually
4. Run `/wz:ping fresh` to initialize

## Step 2: Start Using It

### Add your first task

```
/wz:dump finish the quarterly report by Friday
```

This captures the thought to `_wazeer/Inbox.md` with inferred metadata.

### Run a ping to process the inbox

```
/wz:ping
```

Wazeer will see the inbox item and propose moving it to the appropriate board.

### Build your weekly focus

Open your Work Board or Home Board and add projects and next actions. During your weekly review (Sunday/Monday), move items from Next Actions to `Focus.md > This Week`.

## What Happens During a Ping

1. Wazeer reads all boards, your daily notes, inbox, and git changes since last reconciliation
2. Greets you with a time-appropriate message
3. Shows a compact ASCII snapshot of your Focus board
4. Proposes updates: cards to move, items to add, things to archive
5. Waits for your confirmation before touching boards
6. Updates `_wazeer/Status.md` with a fresh synthesis

## Daily Workflow

```
Morning:  /wz:ping              -> see what's on your plate
          Pull items to Focus   -> start working
During:   /wz:dump <thought>    -> capture anything, don't break flow
          Tell Wazeer updates    -> "BUG-01 is resolved", "TODO-03 is done"
Evening:  /wz:ping              -> reconcile the day, clear Done items
```

## Quick Launch

The `wz` shell alias is the recommended way to start Wazeer daily. It opens Claude Code in the vault, ensures a recurring `/wz:ping` loop is active, and runs an immediate check-in.

The setup flow offers to configure this. To set it up manually:

**bash** (add to `~/.bashrc`) / **zsh** (add to `~/.zshrc`):

```bash
# Wazeer - personal GTD mentor
alias wz='cd ~/Documents/Obsidian/Wazeer && echo "Check CronList for /wz:ping. If not scheduled, read Ping Schedule in CLAUDE.md and set up /loop. Then /wz:ping." | claude'
```

**fish** (add to `~/.config/fish/config.fish`):

```fish
# Wazeer - personal GTD mentor
alias wz 'cd ~/Documents/Obsidian/Wazeer; and echo "Check CronList for /wz:ping. If not scheduled, read Ping Schedule in CLAUDE.md and set up /loop. Then /wz:ping." | claude'
```

Update the vault path to match your directory. After adding the alias, restart your shell or `source` the config file.

The loop interval and weekday-only setting are read from the "Ping Schedule" table in `CLAUDE.md`. Edit that table to change the schedule.

## Setting Up Recurring Pings

You can set up recurring pings manually using Claude Code's `/loop` command:

```
/loop 1h /wz:ping
```

This runs `/wz:ping` on the specified interval, keeping your boards and Status.md fresh throughout the day. The `wz` alias handles this automatically.

## Git Versioning

Commit your vault regularly. Wazeer uses git diffs to detect what changed between pings:

```bash
git add -A && git commit -m "vault backup: $(date '+%Y-%m-%d %H:%M:%S')"
```

Consider setting up auto-commit (via Obsidian Git plugin or a cron job) for hands-free versioning.

## Next Steps

- Read [concepts.md](concepts.md) to understand the GTD flow and board structure
- Read [customization.md](customization.md) to tailor the persona and add your own skills
