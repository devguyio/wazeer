# Getting Started

Set up Wazeer and learn the routines that make it work.

## Setup

Follow the [Quick Start](../README.md#quick-start) in the README to create your vault and install the plugin. The setup wizard walks you through everything interactively:

1. **Vault structure** — creates directories, CLAUDE.md, system files, initializes git
2. **Editor & kanban** — picks your editor, installs a kanban plugin, learns the board format
3. **Version control** — how you want to handle commits
4. **Profile** — your name, role, strengths, challenges, what you need from Wazeer
5. **Persona** — who your advisor is, how they communicate
6. **Boards** — slug prefixes, column layouts, tags
7. **Shell alias** — optional `wz` one-command startup
8. **First ping** — takes the system for a spin

## Your Routines

Wazeer works through three rhythms: a weekly review to plan, a daily routine to stay on track, and ongoing capture throughout the day. Your role is as important as Wazeer's — you update the boards, write daily notes, and make the decisions. Wazeer keeps the system honest and handles the bookkeeping.

### Weekly Review

The anchor of the whole system. Sunday or Monday morning.

Ask Wazeer to go through your inbox and work through it together — clarify each item, decide where it goes, and move it to the right board. Review your Work Board and Home Board: are projects still relevant? Do they have clear next actions? Clear your Done columns and archive what's finished.

Then build your week. Pull items from Next Actions into Focus.md's "This Week" column — these are your commitments for the coming week. Be honest about capacity. Run `/wz:ping` and let Wazeer reconcile everything, update Status.md, and flag anything that keeps sliding week after week.

### Daily

**Morning** — Open your vault and say `/wz:ping`. Wazeer reads your boards, shows what changed since your last check-in, and flags anything overdue. Review the proposals, approve what makes sense. Pull 1-3 items from This Week into Focus — that's your work for today.

**Evening** — Run `/wz:ping` to close out the day. Mark completed items as Done, note anything for tomorrow. Wazeer reconciles the day's changes and refreshes Status.md.

### During the Day

**Capture** — When a thought strikes, `/wz:dump call dentist` sends it to your inbox, tagged and timestamped. This works from any Claude Code session, not just the vault. Don't stop to organize — that's what pings and weekly reviews are for.

**Update** — When work progresses, tell Wazeer: "BUG-01 is resolved", "TODO-03 is blocked on review." Wazeer updates the boards immediately. Move cards yourself when it's quicker — Wazeer picks up the changes on the next ping.

**Daily Notes** — Write your Daily Notes as you go: what you worked on, decisions made, blockers hit, things learned. Daily Notes aren't tasks — they're your log. Wazeer reads them during pings to understand context and spot patterns like late nights or overcommitment.

## What Happens During a Ping

1. Wazeer reads all boards, your daily notes, inbox, and git changes since last reconciliation
2. Greets you with a time-appropriate message
3. Shows a compact ASCII snapshot of your Focus board
4. Proposes updates: cards to move, items to add, things to archive
5. Waits for your confirmation before touching boards
6. Updates `_wazeer/Status.md` with a fresh synthesis

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

## Recurring Pings

You can set up recurring pings using Claude Code's `/loop` command:

```
/loop 1h /wz:ping
```

This runs `/wz:ping` on the specified interval, keeping your boards and Status.md fresh throughout the day. The `wz` alias handles this automatically. The loop interval is read from the "Ping Schedule" table in `CLAUDE.md` — edit that table to change the schedule.

## Git Versioning

Commit your vault regularly. Wazeer uses git diffs to detect what changed between pings:

```bash
git add -A && git commit -m "vault backup: $(date '+%Y-%m-%d %H:%M:%S')"
```

Consider setting up auto-commit (via Obsidian Git plugin or a cron job) for hands-free versioning. Don't worry about clean history — this is a knowledge base, not a codebase.

## Next Steps

- Read [Concepts](concepts.md) to understand the GTD flow and board structure
- Read [Customization](customization.md) to tailor the persona and add your own skills
