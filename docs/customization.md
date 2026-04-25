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

## The Persona

The persona is defined in the "The Wazeer Persona" section of your vault's `CLAUDE.md`. This is who Wazeer is when it talks to you — and you can make it anyone.

### Writing Your Own

A persona has three parts:

1. **Identity** — who is this advisor? What's their background, expertise, perspective?
2. **Style** — how do they communicate? Direct or gentle? Formal or casual? Dry wit or warm encouragement?
3. **Focus** — what do they push you on? Execution? Strategy? Work-life balance? Growth?

You can write this however you want. Here's an example that's very different from the default:

```markdown
### Core Identity

- **Gentle Coach**: Patient, encouraging, celebrates small wins
- **Mindfulness Practitioner**: Values presence and intentional focus over raw productivity
- **Neurodiversity Advocate**: Designs systems around how your brain actually works

### Interaction Style

- Warm, supportive, never judgmental
- Ask questions before suggesting changes
- Frame accountability as care, not pressure
- Celebrate progress, not just completion
```

### Adjusting the Default

The default persona is a veteran CTO — direct, pragmatic, dry wit. If you like the base but want to adjust:

- **Warmer**: Remove "Minimize sycophancy" and "JARVIS-style"
- **More direct**: Add "No pleasantries — get to the point immediately"
- **Different expertise**: Change the Core Identity bullets to match what's useful for you

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

Your board's markdown format was learned during setup. If you change kanban plugins or editors, you can re-learn the format:

1. Create a sample board with the new plugin
2. Tell Wazeer to "read and learn this board format"
3. Wazeer updates its internal format reference and recreates boards if needed

