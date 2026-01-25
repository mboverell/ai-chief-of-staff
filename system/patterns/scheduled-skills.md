# Pattern: Config-Driven Scheduled Skills

Run skills automatically on a schedule, configured via a YAML file in your vault.

## The Pattern

**Separation of concerns:**
- **What** (skill logic and prompt) → lives in vault as `_CoS/_skills/*/SKILL.md`
- **When** (trigger schedule) → config file at `_CoS/_scheduled/config.yaml`

This enables editing schedules and adding new automated tasks without code deploys.

## Config File Structure

Location: `vault/_CoS/_scheduled/config.yaml`

```yaml
scheduled_tasks:
  - name: morning-briefing          # Unique identifier
    skill: daily-briefing           # Folder name in _CoS/_skills/
    schedule: "06:00"               # When to run
    output: telegram                # Where to send result
    enabled: true                   # Toggle on/off

  - name: weekly-review-prompt
    skill: weekly-review
    schedule: "sunday 18:00"
    output: telegram
    enabled: true
```

## Schedule Formats

| Format | Example | When it runs |
|--------|---------|--------------|
| `"HH:MM"` | `"06:00"` | Daily at 6:00 AM |
| `"weekday HH:MM"` | `"sunday 18:00"` | Every Sunday at 6:00 PM |

Times are evaluated in the service's configured timezone (default: Pacific).

## Output Formats

### Telegram

```yaml
output: telegram
```

Sends the skill result directly to the user via Telegram.

### Vault File

```yaml
output: "vault:_briefings/{date}.md"
```

Saves the result to a file in the vault. Supports placeholders:

| Placeholder | Example | Description |
|-------------|---------|-------------|
| `{date}` | `2026-01-24` | YYYY-MM-DD |
| `{datetime}` | `2026-01-24_0600` | YYYY-MM-DD_HHMM |
| `{year}` | `2026` | YYYY |
| `{month}` | `01` | MM |
| `{week}` | `W04` | Week number |
| `{skill}` | `daily-briefing` | Skill name |

## How It Works

1. Background loop checks time every minute
2. Loads config from `_CoS/_scheduled/config.yaml`
3. For each enabled task where schedule matches current time:
   - Loads skill from `_CoS/_skills/{skill}/SKILL.md`
   - Executes skill agentically (Claude + tools)
   - Routes output to configured destination
4. Tracks last run to prevent duplicates

## Adding a Scheduled Task

1. Create the skill in `_CoS/_skills/your-skill/SKILL.md`
2. Add entry to `_CoS/_scheduled/config.yaml`
3. Wait for next vault sync (or trigger manually)

Changes take effect within 5 minutes (next sync cycle).

## Example: Daily Briefing

Skill file at `_CoS/_skills/daily-briefing/SKILL.md`:

```markdown
# Daily Briefing

Generate a morning briefing for today.

## Gather

1. Today's calendar events
2. Top 5 unread emails (by importance)
3. This week's priorities from vault

## Format

Keep it mobile-friendly. Under 500 words.

### Today
[Calendar summary]

### Priority Emails
[Brief summary of what needs attention]

### This Week's Focus
[Reminder of stated priorities]
```

Config entry:

```yaml
- name: morning-briefing
  skill: daily-briefing
  schedule: "06:00"
  output: telegram
  enabled: true
```

## Backward Compatibility

If no config.yaml exists, the service falls back to environment variable mode:

```bash
BRIEFING_ENABLED=true
BRIEFING_TIME=06:00
```

This runs the hardcoded daily briefing at the specified time. Migrate to config.yaml for flexibility.

## Best Practices

**Keep skills focused.** One skill = one output. Don't combine briefing and review into one skill.

**Use descriptive names.** `morning-briefing` is better than `task1`. Names appear in logs.

**Start disabled.** Set `enabled: false` while testing. Enable once confirmed working.

**Archive outputs.** Consider dual-output: Telegram for immediate notification, vault for history.

```yaml
- name: briefing-notify
  skill: daily-briefing
  schedule: "06:00"
  output: telegram
  enabled: true

- name: briefing-archive
  skill: daily-briefing
  schedule: "06:05"
  output: "vault:_briefings/{date}.md"
  enabled: true
```

## Troubleshooting

**Skill not running**
- Check `enabled: true` in config
- Verify skill folder exists in `_CoS/_skills/`
- Check service logs for errors

**Wrong timezone**
- Service uses Pacific by default
- Configure via environment variable if needed

**Duplicate runs**
- Normal after service restart (deduplication resets)
- Once-per-period logic should prevent ongoing duplicates
