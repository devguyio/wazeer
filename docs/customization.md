# Customization

How to make Wazeer yours.

## Your Profile

The most important customization. Open `CLAUDE.md` in your vault and fill in the "Your Profile" section:

```markdown
### About You

- **Name**: Jane Smith
- **Location**: Portland, OR
- **Neurodiversity**: ADHD - benefits from external structure and "what's next?" prompts

### Professional Context

- **Role**: Staff Engineer at Acme Corp
- **Domain**: Platform infrastructure, Kubernetes
- **Experience**: 12 years in backend systems, distributed computing

### Strengths

- Deep systems knowledge
- Good at debugging production issues
- Strong async communication

### Challenges (to optimize over time)

- Context switching between too many projects
- Tendency to overcommit in sprint planning
- Late nights when debugging gets interesting

### What You Need from Wazeer

- Accountability partner - flag when things slide
- "What to do next?" clarity when overwhelmed
- Help maintaining work/life boundaries
```

Be specific. The more context Wazeer has, the better the advice and the more useful the observations in Status.md.

## Faith-Aware Mode

Wazeer can incorporate Islamic scheduling awareness - greeting with Assalamu alaikum, acknowledging Jumu'ah (Friday), using prayer times as natural scheduling checkpoints.

To enable it, find this section in your vault's `CLAUDE.md` and uncomment the line:

```markdown
<!-- FAITH_AWARENESS: Uncomment the line below to enable faith-aware greetings and scheduling.
     Wazeer will use greetings like "Assalamu alaikum", acknowledge prayer times as natural
     scheduling checkpoints, and note occasions like Jumu'ah (Friday).
- Faith-aware without being preachy
-->
```

Change it to:

```markdown
<!-- FAITH_AWARENESS: enabled -->
- Faith-aware without being preachy
```

This is opt-in. If you don't uncomment it, Wazeer uses neutral, time-appropriate greetings.

You can also adjust the Wazeer persona to reflect your own faith tradition or remove faith references entirely - the persona section in `CLAUDE.md` is fully editable.

## Persona Adjustments

The Wazeer persona is defined in the "The Wazeer Persona" section of `CLAUDE.md`. You can modify:

### Core Identity

Change the background to match what's useful for you:

```markdown
### Core Identity

- **Hands-On Technical Expert**: Deeply technical, coding almost daily.
- **Balanced Leader**: Mastered the balance between individual contributor work and leadership.
- **Financial Literacy**: Knowledgeable about personal finance and wealth building.
- **Neurodiversity Specialist**: Expert in productivity for neurodivergent professionals.
```

### Interaction Style

Adjust the communication style:

```markdown
**Interaction Style:**
- Collaborate, don't preach
- Accountability partner
- High signal, no fluff
- Minimize sycophancy
- JARVIS-style: competent, brief, dry wit allowed
```

If you want a warmer tone, remove "Minimize sycophancy" and "JARVIS-style". If you want it even more direct, add "No pleasantries - get to the point immediately."

## Custom Tags

Tags are defined by convention, not configuration. Start using a tag and it exists. Common patterns:

**By project:**
```
#project-alpha #project-beta #infrastructure
```

**By team:**
```
#platform-team #frontend-team #data-team
```

**By urgency:**
```
#urgent #can-wait #someday
```

Define whatever makes sense for your work. The board-management skill will pick up your conventions.

## Slug Prefixes

Card slug prefixes (e.g. TODO, REV, BUG) are defined in the "Card Slugs" table in `CLAUDE.md`. Edit that table directly. The board-management skill reads from that table when creating cards.

## Adding Project Folders

As you start tracking projects, create subfolders:

```
Projects/
  TODO/         - complex task companion notes (auto-created by board-management)
  BUG/          - bug tracking companion notes (auto-created by board-management)
  my-project/   - your project files
```

The subfolders under `Projects/` match your slug prefixes. They are created automatically when Wazeer makes companion notes for cards of that type.

## Adjusting the Vault Structure

During reconciliation, `/wz:ping` runs a tidy pass that enforces a specific folder structure. The structure is defined in the tidy skill's instructions. If you want to add new top-level folders that shouldn't be flagged as strays, tell Wazeer and it will note them as system folders.

For example, to add a `References/` folder for long-lived reference material:

1. Create the folder
2. Tell Wazeer: "References/ is a system folder, don't flag it in tidy"

## Board Format

Your board's markdown format was learned during setup and saved to `_wazeer/board-format.md`. If you change kanban plugins or editors, you can re-learn the format:

1. Create a sample board with the new plugin
2. Tell Wazeer to "read and learn this board format"
3. Wazeer updates `_wazeer/board-format.md` and recreates boards if needed

## Git Integration

Wazeer uses git diffs to detect changes between pings. For best results:

- Commit regularly (the Obsidian Git plugin can auto-commit)
- Use descriptive commit messages or auto-generated timestamps
- Don't worry about clean history - this is a knowledge base, not a codebase
