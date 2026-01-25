# AI Chief of Staff

A personal AI that synthesizes your notes, calendar, and email into executive-level insight. Built on plain text files you own, with read-only access to external data.

## The Key Ideas

- **Own your context layer.** All memory lives in plain text files, not vendor systems. Switch models anytime.
- **Skills are procedures, not prompts.** A skill is a multi-step workflow the AI executes, not context for a response.
- **Read-first, write-cautious.** Start with read-only access. Earn trust before expanding permissions.

## What's Working (Week 6)

Six weeks of daily use. The system now includes:

- **Weekly review** — Synthesizes meetings, calendar, email, and git commits. Catches open loops and slippage.
- **Daily briefing** — Arrives at 6 AM via Telegram. Calendar, priority emails, weekly focus.
- **Mobile access** — Query from phone via Telegram bot. Read-only calendar/email.
- **Scheduled skills** — Config-driven automation. Skills run on schedule without asking.

See [NOTES.md](NOTES.md) for the full journey from Week 0 to now.

## Try It

**Quickest path:** Run the weekly review skill locally with Claude Code. No integrations needed.

→ [QUICKSTART.md](QUICKSTART.md) — 30 minutes to your first weekly review

## Architecture

```
Your Vault (Obsidian)
    │
    ├── _CoS/_skills/          ← Skill procedures (weekly-review, daily-briefing)
    ├── _CoS/_context/         ← Objectives, patterns, key people
    ├── _CoS/_scheduled/       ← Automation config
    │
    └── Content                ← Meetings, priorities, reflections
            │
            ▼
      AI Agent (Claude)        ← Reads vault, executes skills, writes output
```

Full architecture docs: [system/SCHEMA.md](system/SCHEMA.md)

## Learn More

- [NOTES.md](NOTES.md) — How this evolved (Week 0 → Week 6)
- [QUICKSTART.md](QUICKSTART.md) — 30-minute adoption guide
- [system/](system/) — Architecture, skills, patterns
- [templates/](templates/) — Starter files for your vault

## License

MIT — use freely, adapt as needed.
