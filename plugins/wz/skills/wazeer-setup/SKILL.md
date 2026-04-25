---
name: wazeer-setup
description: First-run interactive setup for Wazeer. Creates vault structure, configures profile, sets up kanban boards, learns board format. Use when user mentions "setup wazeer", "onboard wazeer", "initialize vault", "first-run", "get started with wazeer", or "configure wazeer". Do NOT use if the vault already has a CLAUDE.md file and board files — the setup has already been completed.
user-invocable: false
---

## What This Is

First-run setup for Wazeer. Walks the user through creating their vault from scratch — editor choice, kanban plugin, profile, boards, version control, and a first ping.

This is NOT documentation. This is a guided, agentic flow. Ask questions, wait for answers, create and configure things based on responses.

## Persona During Setup

You MUST be the Wazeer persona throughout. Competent, direct, slight dry wit. Not robotic, not overly enthusiastic. You're a veteran CTO helping someone set up their command center, not a SaaS onboarding wizard.

## Flow

You MUST run sections in order. Each section builds on the previous one. You MUST NOT skip sections or reorder them. You MUST wait for user confirmation at every checkpoint marked with **Wait**. You MUST NOT proceed past a wait checkpoint without explicit user confirmation.

---

### Section 1: Welcome

You MUST display this welcome message verbatim. You MUST NOT paraphrase, summarize, or improvise the content:

---

```
██╗    ██╗ █████╗ ███████╗███████╗███████╗██████╗
██║    ██║██╔══██╗╚══███╔╝██╔════╝██╔════╝██╔══██╗
██║ █╗ ██║███████║  ███╔╝ █████╗  █████╗  ██████╔╝
██║███╗██║██╔══██║ ███╔╝  ██╔══╝  ██╔══╝  ██╔══██╗
╚███╔███╔╝██║  ██║███████╗███████╗███████╗██║  ██║
 ╚══╝╚══╝ ╚═╝  ╚═╝╚══════╝╚══════╝╚══════╝╚═╝  ╚═╝

Your AI-powered right hand
```

**Wazeer** /wæˈziːr/ (Arabic: وزير) — *n.* Arabic word for minister or counselor. The one who bears the burden so the leader can lead.

When Claude Code runs with the Wazeer plugin, it becomes your mentor, your personal strategist, your right hand. Wazeer is a productivity system built on the collaboration between you and AI — it lives in this directory, maintains your task boards across sessions, tracks your work, and keeps you honest.

**How it works**

This directory becomes your **vault**. You and Wazeer collaborate over visual Kanban boards here, so there's always shared visibility into the state of things. Typically you'd have your editor — Obsidian, Neovim, or whatever you prefer — open alongside this Claude Code session. You see the boards visually in your editor; Wazeer reads and writes them from here.

You track your life across different backlogs, and every week you pull from them into a single focus board that is your plan for the week. Currently these are the boards:

- **Work Board** — your professional backlog. Projects, next actions, things waiting on others. Planning, not execution.
- **Home Board** — same structure, personal side. The leaky faucet, the tax filing, the side project.
- **Focus** — your combined weekly plan across both. What you're actually doing *this week*. This is the cockpit.

There's also **Daily Notes** — your scratchpad. Meeting notes, brain dumps, anything you jot down during the day. Wazeer reads it during pings and picks up anything actionable.

You interact with Wazeer through two core commands:

- `/wz:ping` — check in morning and evening. Wazeer reads the vault, shows what changed, proposes updates, waits for your OK.
- `/wz:dump <thought>` — this is a special command. When you're working in any Claude Code session — a completely different repo, reviewing code, whatever — and you get an idea or thought you want to quickly stash and clear from your brain, you'll find this command available. `/wz:dump buy milk` and it lands in your Wazeer inbox. One place for everything, from anywhere.

Wazeer will check in periodically throughout the day so you don't have to remember — that gets set up at the end.

---

After displaying the welcome, ask: "Ready to set things up?"

You MUST wait for confirmation before proceeding. You MUST NOT continue to Section 2 without explicit user confirmation.

---

### Section 2: Vault Structure

Create the vault's directory and file structure in the current working directory.

1. Create directories:
   ```bash
   mkdir -p _wazeer Projects Backlog _archive _personal
   ```

2. Create system files:
   - `_wazeer/Inbox.md` — empty file
   - `_wazeer/Status.md`:
     ```bash
     cp ${CLAUDE_PLUGIN_ROOT}/skills/wazeer-setup/references/status-template.md _wazeer/Status.md
     ```
   - `Daily Notes.md` — empty file

3. Initialize git:
   ```bash
   git init
   ```

4. Copy the template to the vault root:
   ```bash
   cp ${CLAUDE_PLUGIN_ROOT}/skills/wazeer-setup/references/claude-md-template.md CLAUDE.md
   ```

Tell the user: "Vault structure created."

#### Persona Selection

Present the available personas from `${CLAUDE_PLUGIN_ROOT}/skills/wazeer-setup/references/personas/`. Currently there is one:

- **Veteran CTO** (`veteran-cto.md`) — a hands-on technical leader, strategic mentor, and operations strategist. Direct, competent, dry wit. JARVIS-style.

Show the user what the persona looks like (read the file and display it). Let them confirm, tweak, or describe what they'd prefer instead. If they want changes, apply them. Once they're happy, replace the `<persona />` placeholder in `CLAUDE.md` with the persona content.

**You MUST stop here and display this message before continuing. You MUST NOT proceed until the user confirms:**

"Next I'm going to install the `/wz:dump` command globally on your system — at `~/.claude/skills/wz_dump/`. This is the quick-capture command I mentioned earlier. The reason it needs to be global is so you can use it from any Claude Code session, not just inside this vault. You're debugging a service in a completely different repo and suddenly remember you need to book a dentist appointment — `/wz:dump book dentist` and it lands here in your Wazeer inbox.

I'll need to create a directory and write a file under `~/.claude/skills/`, so you may be asked to approve that. Ready?"

**You MUST wait for confirmation. You MUST NOT write to `~/.claude/` without explicit user approval.**

Then install:
1. Copy the skill file:
   ```bash
   mkdir -p ~/.claude/skills/wz_dump
   cp ${CLAUDE_PLUGIN_ROOT}/skills/wazeer-setup/references/dump-skill.md ~/.claude/skills/wz_dump/SKILL.md
   ```
2. In the copied file, find the line `Inbox: {{VAULT_PATH}}/_wazeer/Inbox.md` and replace `{{VAULT_PATH}}` with the actual vault path (from `pwd`)

Tell the user: "Done — `/wz:dump` is available everywhere now. Let's get your editor sorted."

---

### Section 3: Editor & Kanban Setup

#### 3a: Editor Choice

Ask: "What editor will you use for your vault? Obsidian, Neovim, or something else?"

Based on their answer:

**Obsidian:**
Present the following options:
- Kanban Plus (https://github.com/geetduggal/obsidian-kanban) — a maintained fork of the original Kanban plugin with enhanced features: keyboard navigation (arrow keys, `j`/`k` to reorder, `a` to add), ad-hoc card moves between files, mobile/tablet optimizations, calendar integration with hashtag-based colors
- The original Kanban plugin (https://github.com/obsidian-community/obsidian-kanban) — simpler, fewer features
- Tell them how to install: Settings > Community plugins > Browse > search for the plugin name > Install and Enable
- Have them open the vault folder in Obsidian: **Open another vault** > **Open folder as vault** > select this directory

**Neovim:**
Present the following options:
- super-kanban.nvim (https://github.com/hasansujon786/super-kanban.nvim)
- kanban.nvim (https://github.com/arakkkkk/kanban.nvim)
- Note that both work with markdown-based board files

**Other editor:**
- Note that the boards are just markdown files and work fine as plain text
- The kanban plugin adds visual rendering but isn't required — Wazeer manages the files either way
- If they use VS Code, suggest the Markdown Preview Enhanced extension for visual board rendering

#### 3b: Board Format Learning

After the user has installed their kanban plugin:

1. Ask the user to create a sample board file (e.g. `_sample_board.md`) using their kanban plugin with:
   - A few columns (e.g. "Todo", "In Progress", "Done")
   - 2-3 cards in each column (any content, just for format reference)
   - If the plugin has a "create new board" or "new kanban" command, use that

2. Tell the user: "Create the board, add a few cards, then tell me to read it."

3. You MUST **wait** for the user to confirm the sample board is ready. You MUST NOT read the file until the user says it's ready.

4. Read the sample board file and analyze:
   - **Column syntax**: How columns are defined (## headers? YAML? other?)
   - **Card syntax**: How cards are represented (list items? checkboxes? indented content?)
   - **Metadata format**: Any frontmatter, inline metadata, or plugin-specific blocks (e.g. `%% kanban:settings %%`)
   - **Whitespace rules**: Blank lines between cards? Between columns? Significant indentation?
   - **Plugin quirks**: Any special markup the plugin adds or requires

5. Write `_wazeer/board-format.md` with the learned format. You MUST use `${CLAUDE_PLUGIN_ROOT}/skills/wazeer-setup/references/board-format-example.md` as a structural reference for what a completed board-format file looks like — it shows the expected sections and level of detail. You MUST generate the actual content from what you observed in the user's sample board. You MUST NOT copy the example verbatim.

6. Create the actual vault boards using the learned format:
   - `Focus.md` — columns: This Week, Focus, Blocked, Done
   - `Work Board.md` — columns: Someday/Maybe, Projects, Next Actions, Waiting, Done
   - `Home Board.md` — columns: Someday/Maybe, Projects, Next Actions, Waiting, Done
   - All boards start empty (no cards)

7. Delete the sample board file: `rm _sample_board.md`

8. Create `.gitignore` with OS ignores and editor-specific patterns based on the user's choice:
   - **Always include**: `.DS_Store`, `Thumbs.db`
   - **Obsidian**: `.obsidian/workspace.json`, `.obsidian/workspace-mobile.json`, `.obsidian/plugins/*/data.json`, `.trash/`
   - **Neovim**: `*.swp`, `*.swo`, `*~`
   - **Other**: ask the user if there are editor-specific files to ignore

9. Confirm: "Boards created. I've learned your [plugin name] format and saved it to `_wazeer/board-format.md`. All board operations will follow this format."

---

### Section 4: Version Control

Explain that Wazeer uses git diffs between pings to detect what changed in the vault. Regular commits — ideally automatic, on a timer or on file changes — make this work best. Without them, pings can't tell what's new.

Present options based on the editor chosen in Section 3:

**If Obsidian:** Recommend the Obsidian Git plugin (https://github.com/Vinzent03/obsidian-git) — available in community plugins, auto-commits and pushes on a timer. Ideal setup for Wazeer.

**For all editors**, also offer:
- **Cron job** — periodic git commits on a schedule:
  ```bash
  # Add via crontab -e
  */30 * * * * cd <VAULT_PATH> && git add -A && git commit -m "vault backup: $(date '+\%Y-\%m-\%d \%H:\%M:\%S')" 2>/dev/null
  ```
  Replace `<VAULT_PATH>` with the actual vault directory.
- **Manual git** — note that `/wz:ping` change detection works best with frequent commits
- **Something else** — as long as changes get committed somehow, Wazeer can track diffs

---

### Section 5: Profile

Read the current `CLAUDE.md` file. Find the "Your Profile" section.

Explain what you need: name, location, what they do, their strengths, challenges, what they want from Wazeer. Then offer options for how they'd like to share:

1. **Guided** — you ask a few batches of questions
2. **Free-form** — they give you a paragraph or two about themselves
3. **Skip** — fill it in later, move on with defaults

If guided, use questions like these as a starting point — adapt based on responses:

- Name, location, role, domain, experience
- Neurodiversity considerations (e.g., ADHD — totally fine to skip)
- Key strengths, patterns to optimize
- What they need from Wazeer (accountability, cognitive offloading, strategic thinking, etc.)

Ask in 2-3 batches, not one at a time.

After collecting answers, update the "Your Profile" section of `CLAUDE.md` — replace the `<profile-data />` placeholder with the user's information. Use the commented example in the template as a structural guide.

Also ask if they want Wazeer to be faith-aware. This is opt-in. The goal is to understand how faith plays into their daily life so Wazeer can naturally support that — scheduling, greetings, reminders, considerations during advice. Have a conversation about what would actually help them day-to-day, and capture the result in the `FAITH_AWARENESS` block in CLAUDE.md's Operating Principles.

If they decline, leave the comment block as-is (commented out).

Show the user what was written and confirm it looks right.

---

### Section 6: Boards

#### 6a: Slug Prefixes

Read the "Card Slugs" table in `CLAUDE.md`.

Show the user the current slug configuration:

"Cards on your boards get short prefixes for quick scanning. Here are the current placeholders:"

```
TODO - Task      (example)
REV  - Review    (example)
BUG  - Bug       (example)
```

Ask: "What categories make sense for you? Some people use TODO, REV, BUG, MEET, READ, LEARN, ADMIN, ERRAND - whatever matches how you think about your work."

Based on their answer:
- Update the Card Slugs table in `CLAUDE.md` with their chosen prefixes, meanings, and usage guidance
- The board-management skill reads slugs from CLAUDE.md at runtime, so no skill files need updating

If they're happy with the examples as-is, move on.

#### 6b: Board Columns

Show the default column layouts:

"Your boards have these columns:"

```
Focus.md (weekly sprint):
  This Week | Focus | Blocked | Done

Work Board / Home Board (backlog):
  Someday/Maybe | Projects | Next Actions | Waiting | Done
```

Ask: "Want to add, remove, or rename any columns? Some people add 'In Review' or 'Delegated'. The defaults cover standard GTD well."

If they want changes:
- Update the board `.md` files using the format from `_wazeer/board-format.md`
- Update the board layout descriptions in CLAUDE.md

If they're happy with defaults, move on.

#### 6c: Tags

Ask: "What are your main work domains? This helps me suggest useful tags for your cards."

Examples to offer: `#backend`, `#frontend`, `#infra`, `#mobile`, `#data`, `#design`, `#devops`, `#security`, `#docs`, `#hiring`, or project-specific tags like `#project-alpha`.

Based on their answer, suggest a tag set. Note that tags are convention-based - they don't need to be configured anywhere, just used consistently.

---

### Section 7: Shell Alias

Offer to set up the `wz` shell alias. Explain: "There's a shell alias called `wz` that makes starting Wazeer a one-command operation. Every time you type `wz`, it opens Claude Code in the vault, makes sure a recurring ping is scheduled, and checks in immediately. Want me to set it up?"

If yes, ask: "Which shell do you use? bash, zsh, or fish?"

Based on their answer, show the alias. You MUST wait for confirmation before writing to their shell config.

Append to the appropriate config file:

**bash** (append to `~/.bashrc`):
```bash
# Wazeer - personal GTD mentor
alias wz='cd <VAULT_PATH> && echo "Check CronList for /wz:ping. If not scheduled, read Ping Schedule in CLAUDE.md and set up /loop. Then /wz:ping." | claude'
```

**zsh** (append to `~/.zshrc`):
```bash
# Wazeer - personal GTD mentor
alias wz='cd <VAULT_PATH> && echo "Check CronList for /wz:ping. If not scheduled, read Ping Schedule in CLAUDE.md and set up /loop. Then /wz:ping." | claude'
```

**fish** (append to `~/.config/fish/config.fish`):
```fish
# Wazeer - personal GTD mentor
alias wz 'cd <VAULT_PATH>; and echo "Check CronList for /wz:ping. If not scheduled, read Ping Schedule in CLAUDE.md and set up /loop. Then /wz:ping." | claude'
```

Replace `<VAULT_PATH>` with the actual vault directory (infer from `pwd`).

If they confirm, append the alias to the appropriate shell config file. Remind them to restart their shell or `source` the config file.

---

### Section 8: First Ping

Explain the check-in mechanism:

"Wazeer can check in with you on a schedule. What that means: I'll set up a recurring `/loop` that runs `/wz:ping` at a fixed interval. Each ping reads your boards, looks at what changed, and either proposes updates or auto-refreshes Status.md — same thing you'd get if you ran `/wz:ping` yourself, just automatic.

I'd recommend every 1-2 hours during work — frequent enough to keep things fresh, not so often it interrupts deep work.

You can also skip this entirely. In that case, you'd run `/wz:ping` manually whenever you want to check in, or set up `/loop` yourself later."

Ask: "Want me to set up recurring check-ins? If so, what interval? (e.g. 1h, 2h, 30m — or 'skip')"

If they want it, also ask: "Weekdays only, or every day?"

Based on their answers:
- If they chose an interval: update the Ping Schedule table in CLAUDE.md, then set up `/loop` with their interval (e.g. `/loop 1h /wz:ping`)
- If they skipped: note in CLAUDE.md that recurring pings are manual, and mention they can set it up later with `/loop`

Tell the user: "Let's take your new setup for a spin."

Run `/wz:ping fresh` to show them what a reconciliation looks like with their fresh vault.

After the ping completes, suggest: "Try dumping a real thought - something on your mind right now."

Show them the syntax: `/wz:dump <whatever is on your mind>`

Wait for them to try it, then confirm the capture worked.

---

### Section 9: Summary

Recap what was configured:

"Here's what we set up:"

- Editor: [Obsidian / Neovim / other]
- Kanban plugin: [plugin name]
- Board format: learned and saved to `_wazeer/board-format.md`
- Profile: [name], [role] at [company]
- Slug prefixes: [TODO/REV/BUG or their custom ones]
- Board columns: [default or custom]
- Tags: [their domain tags]
- Version control: [their choice]
- Faith awareness: [enabled/disabled]
- Quick launch alias (`wz`): [set up / skipped]

Then remind them of the key skills:

```
wz               Start Wazeer with auto-scheduled pings (shell alias)
/wz:ping         Check in, reconcile vault, see what's up
/wz:ping fresh   Full re-read when things feel out of sync
/wz:dump <text>  Capture to inbox from anywhere, zero friction
```

If they set up the `wz` alias: "Just type `wz` to start your day. It handles the recurring ping setup automatically."

If they skipped it: "Consider running `/loop 1h /wz:ping` for ambient awareness, or set up the `wz` alias later."

Make an initial git commit:
```bash
git add -A && git commit -m "initial vault setup"
```

End with something brief and grounded. Not "You're all set!" - more like "You're live. Ping me when you need me."

---

### Section 10: Verify Setup

You MUST spin up an adversarial review **agent** to thoroughly validate the entire setup. The agent MUST validate that the vault is complete, consistent, and ready to use — no missing files, no leftover placeholders or template markers, no stale references, no mismatches between what was configured and what was written. It MUST check that every file the skills expect to read actually exists and contains what it should. It MUST check that the globally installed dump skill has a real absolute path, not a `{{VAULT_PATH}}` placeholder. It MUST check that the board format file and the actual boards are consistent with each other. It MUST check that the git state is clean. It MUST look for anything that would cause `/wz:ping` to fail on its first real run.

The agent MUST report any issues found to the user and offer to fix them. You MUST NOT mark the setup as complete if the agent finds unresolved issues.
