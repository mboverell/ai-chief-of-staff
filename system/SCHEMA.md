# SCHEMA.md

How the vault is organized. Reference to locate files and understand conventions.

---

## Vault Structure

```
YourVault/
├── AGENTS.md              # Global config: who I am, rules, skill index
├── SCHEMA.md              # This file: vault organization
│
├── _CoS/                  # Chief of Staff system
│   ├── _context/          # Background context (load on demand)
│   │   ├── objectives.md
│   │   ├── how-i-work.md
│   │   └── key-people.md
│   ├── _skills/           # Modular agent capabilities
│   │   └── weekly-review/
│   │       └── SKILL.md
│   ├── _state/            # Agent-writable state files
│   └── _templates/        # Canonical content templates
│
├── _priorities/           # Weekly planning notes
│   └── YYYYMM • WXX • Weekly Prios.md
│
├── Meetings/              # Meeting exports
│   └── YYYY/
│       └── *.md
│
├── Projects/              # One folder per project
│   └── project-name/
│       └── [working notes]
│
├── People/                # Relationship notes
├── Readwise/              # Reading highlights (auto-imported)
├── Reflections/           # Periodic self-reflection
│   └── weekly/
│       └── *.md
└── _inbox/                # Quick capture, unprocessed
```

---

## Directory Purposes

| Directory | Purpose | When to Load |
|-----------|---------|--------------|
| `_CoS/_context/` | Personal context, objectives, working style | Skills need background on who I am |
| `_CoS/_skills/` | Task-specific procedures | Task matches a skill description |
| `_CoS/_templates/` | Canonical structures for content types | Creating new content |
| `_CoS/_state/` | Agent-writable state (actions, etc.) | Tracking persistent state |
| `_priorities/` | Weekly priorities and daily scratch | Analyzing time, reviewing week |
| `Meetings/` | Meeting summaries | Searching discussions, decisions |
| `Projects/` | Active project folders | Working on specific projects |
| `People/` | Relationship context | Meeting prep |
| `Readwise/` | Reading highlights | Researching topics |
| `Reflections/` | Self-reflection entries | Historical patterns, long-term context |
| `_inbox/` | Unprocessed quick captures | Processing inbox |

---

## File Naming Conventions

| Content Type | Pattern | Example |
|--------------|---------|---------|
| Weekly priorities | `YYYYMM • WXX • Weekly Prios.md` | `202501 • W03 • Weekly Prios.md` |
| Meetings | Title-based (from export tool) | `Team Standup-2025-01-15_10-00.md` |
| Projects | Folder per project | `Projects/2025 - Website Redesign/` |

---

## Meeting Frontmatter

All meeting files should have this structure:

```yaml
---
title: "Meeting Title"
created: 2025-01-15T10:00:00.000Z   # ← Use this for date queries
updated: 2025-01-15T10:30:00.000Z
attendees:
  - email@domain.com                 # ← Map via _CoS/_context/key-people.md
---
```

**Key:** Use `created:` field for date-based filtering, not filename.

---

## Output Conventions

### Replaceable Blocks

Agent outputs within human notes use stable markers:

```markdown
<!-- AGENT:skill-name:START -->
## 🤖 [Skill Name] (generated YYYY-MM-DD)

[Content]

<!-- AGENT:skill-name:END -->
```

Re-running a skill replaces content between markers.

### State Files

Files in `_CoS/_state/` can be fully overwritten by agents. This is the only exception to the append-only rule.

---

## How to Find Things

| I need... | Look in... | Filter by... |
|-----------|------------|--------------|
| Historical reflections | `Reflections/weekly/` | Date in filename |
| This week's priorities | `_priorities/` | Current week in filename |
| Meetings this week | `Meetings/YYYY/` | `created:` frontmatter |
| Meetings with [person] | `Meetings/YYYY/` | `attendees:` frontmatter |
| Annual objectives | `_CoS/_context/objectives.md` | — |
| Project notes | `Projects/[name]/` | — |
| Action items | Meetings, or `_CoS/_state/open-actions.md` | Search: "[ ]", "Next Steps" |
| Reading on topic | `Readwise/` | Full-text search |
| How I work | `_CoS/_context/how-i-work.md` | — |
| Person context | `_CoS/_context/key-people.md` | Email address |

---

*Last updated: YYYY-MM-DD*
