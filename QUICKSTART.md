# Quickstart: Weekly Review in 30 Minutes

The fastest path to value: run the weekly review skill locally with Claude Code. No API keys, no deployments, no integrations. Just your vault and a Claude session.

## What You'll Get

A synthesized weekly review that:
- Summarizes what you worked on this week
- Identifies open loops (commitments that need follow-up)
- Surfaces priorities that are slipping
- Asks questions to prompt reflection

## Prerequisites

- Obsidian vault (or any folder of markdown files)
- Claude Code (Cursor, VS Code extension, or CLI)
- Meeting notes, daily logs, or project files in your vault

## Step 1: Set Up Vault Structure (5 minutes)

Create the skills folder in your vault:

```
your-vault/
├── _CoS/
│   └── _skills/
│       └── weekly-review/
│           └── SKILL.md
```

## Step 2: Create the Weekly Review Skill (10 minutes)

Create `_CoS/_skills/weekly-review/SKILL.md` with this content:

```markdown
# Weekly Review

Generate a weekly review synthesizing the past 7 days of activity.

## Data to Gather

1. **Meeting notes** - Search for files modified in the past 7 days containing meeting notes
2. **Daily logs** - Check for daily notes or journal entries
3. **Project updates** - Look for project files with recent modifications
4. **Completed tasks** - Find any completed items or shipped work

## Analysis

After gathering data, analyze:

1. **Time allocation** - What got the most attention this week?
2. **Open loops** - What commitments were made that need follow-up?
3. **Priorities** - Are stated priorities getting time, or is drift happening?
4. **Blockers** - What's stuck or waiting on others?

## Output Format

Structure the review as:

### This Week

[2-3 sentence summary of the week's theme]

### Key Activities
- [Bullet list of significant meetings, work, or events]

### Open Loops
- [Commitments made that need follow-up]
- [Questions waiting for answers]

### Observations
- [1-2 patterns noticed]
- [Any misalignment between priorities and time spent]

### Looking Ahead
- [1-2 questions to consider for next week]
```

## Step 3: Run the Skill (15 minutes)

Open your vault in Claude Code (Cursor, VS Code, or terminal).

Prompt Claude:

```
Read the skill file at _CoS/_skills/weekly-review/SKILL.md and execute it.
Search my vault for meeting notes and activity from the past 7 days,
then generate a weekly review following the skill instructions.
```

Claude will:
1. Read the skill file
2. Search your vault for relevant content
3. Synthesize findings into a review
4. Output the formatted result

## What Makes This Work

**Skills are procedures, not prompts.** The SKILL.md file isn't a system prompt—it's instructions Claude follows step by step. Claude has access to your vault files and can search, read, and synthesize.

**Your data stays local.** Claude reads your vault files directly. No data uploaded anywhere.

**Iterate on the skill.** If the review isn't useful, edit SKILL.md. Add sections. Remove sections. Make it work for your workflow.

## Next Steps

Once this works:

1. **Add more skills** — Create `_CoS/_skills/daily-briefing/SKILL.md` for morning context
2. **Add calendar** — Set up Google OAuth for scheduling awareness
3. **Go mobile** — Deploy as Telegram bot for phone access
4. **Automate** — Configure scheduled execution for hands-off operation

## Troubleshooting

**"No relevant files found"**

Your vault might use different naming conventions. Tell Claude explicitly:
- "My meeting notes are in the Meetings/ folder"
- "Daily notes are named YYYY-MM-DD.md"
- "Projects are in Projects/ subfolder"

**Review is too generic**

Add more specific instructions to SKILL.md:
- List the projects you want tracked
- Specify what "open loop" means in your context
- Add prompts for the specific questions you want answered

**Review is too long**

Add constraints to SKILL.md:
- "Keep the review under 500 words"
- "Focus on the top 3 priorities only"
- "Skip sections with no relevant content"
