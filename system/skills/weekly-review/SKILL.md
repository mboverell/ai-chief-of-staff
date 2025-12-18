---
name: weekly-review
description: Weekly reflection that synthesizes meetings, patterns, and reading into coach-level insights. Not admin playback — trajectory and challenge.
version: 2.1
---

# Weekly Review Skill

## Purpose

Step back from the week with the clarity of an elite executive coach. Synthesize meetings, priorities, multi-week trajectory, and reading into direct observations that surface patterns and ask the uncomfortable questions.

**This is not admin playback.** It's a mirror that shows what's happening across weeks, and challenges thinking.

## When to Use

- End of week (Friday or Sunday)
- When asked to "review my week" or "how did my week go"
- When asked about trajectory, alignment, or patterns

## Prerequisites

**The user must provide the current date** so you can determine:
- Which week to analyze (ISO week number)
- Which weekly priority file to find
- Which meetings fall within the week

## Inputs Required

### Core Inputs

1. **This week's priority note**
   - Location: `_priorities/YYYYMM • WXX • Weekly Prios.md`
   - Contains stated Work and Personal priorities

2. **This week's meetings**
   - Location: `Meetings/YYYY/*.md`
   - Filter by: `created:` frontmatter field falls within the week (Mon-Sun)

3. **Objectives reference**
   - Location: `_CoS/_context/objectives.md`
   - Contains annual/quarterly goals for alignment checks

4. **Working patterns**
   - Location: `_CoS/_context/how-i-work.md`
   - Known tendencies to flag

### Trajectory Inputs

5. **Recent reflection entries**
   - Location: `Reflections/weekly/*.md`
   - Read the **last 2-3 weeks of entries**
   - These reveal narrative arc, emotional state, recurring themes

### Reading Inputs

6. **Recently saved articles**
   - Location: `Readwise/Articles/*.md`
   - Filter by: `saved:` field in YAML frontmatter (within last 7-10 days)
   - What someone saves reveals what's on their mind

### Feedback Loop Input

7. **Previous week's reflections** (if available)
   - Location: Previous week's priority note, look for `<!-- REFLECTIONS:START -->` block
   - Contains corrections and context from the user about the prior review
   - Use these to calibrate the current review

## Process

### Step 1: Gather Data

1. Parse the current date → week number (ISO week), current quarter
2. Find the weekly priority note for that week
3. Read `_CoS/_context/objectives.md` for quarterly goals
4. Read `_CoS/_context/how-i-work.md` for known patterns
5. **Read recent reflection entries** — find the last 2-3 weeks
6. List and read all meetings from `Meetings/YYYY/` where `created:` falls within Mon-Sun
7. **Find recently-saved articles** — filter by `saved:` frontmatter within last 7-10 days
8. **Check for previous week's reflections** — find the prior week's priority note, look for `<!-- REFLECTIONS:START -->` block

### Step 2: Analyze

1. **Snapshot the week:**
   - Count meetings by theme/category
   - Count open loops extracted
   - Assess priority hit rate (how many weekly priorities got meaningful progress?)

2. **Trace the trajectory (2-3 weeks):**
   - What themes are persisting? What's shifting?
   - Is this week an anomaly or part of a trend?
   - How does emotional/energy state compare to recent weeks?

3. **Synthesize narrative observations:**
   - Not "Priority X: ❌ No progress" → instead: "The exploration sprint appears frozen while urgent requests consume bandwidth."
   - Connect dots between meetings, priorities, and stated goals
   - Name what's happening without judgment, but without softening
   - Reference known patterns from `how-i-work.md` when they show up

4. **Surface what you're consuming:**
   - What did you save to read this week? Any themes?
   - Does your reading align with your stated priorities?
   - Sometimes what we read reveals what we're not saying out loud.

5. **Extract open loops:**
   - Action items, pending decisions, follow-ups committed
   - Include source file for each item

6. **Formulate coach questions:**
   - What would an elite exec coach ask, given this data?
   - What question are you probably avoiding?
   - What would you tell a friend if they described this week to you?

### Step 3: Generate Output

Produce the review using the replaceable block format. Append to the weekly priority note.

## Output Format

**Important formatting notes:**
- Add `---` horizontal rule before the start marker
- Use bullets over tables where possible (less visual weight)
- Lead with narrative, not checklists
- End with direct questions, not summaries

```markdown
---

<!-- AGENT:weekly-review:START -->
## 🔭 Weekly Review (generated YYYY-MM-DD)

### The Week at a Glance
- **Meetings:** X (themes: [top 2-3 categories])
- **Open loops:** X items surfaced
- **Priorities:** X of Y got meaningful progress
- **Energy signal:** [If discernible: "Running hot", "Depleted", "Stable", "Rebuilding"]

### Trajectory (last 2-3 weeks)
[2-3 sentences synthesizing the arc from recent reflection entries. What's the trend? Is this week a continuation, a break, or an inflection point?]

### What I'm Seeing
[This is the heart of the review. 3-4 paragraphs of narrative synthesis. Write in coach voice — direct, observational, connecting dots.]

Cover:
- How the week's activity maps to stated priorities (weekly, quarterly, annual)
- What got attention vs. what got ignored
- Patterns from `how-i-work.md` showing up
- Any emerging tensions between what you say you want and what you're doing

[Don't soften. Don't hedge excessively. Be the coach who sees clearly.]

### What You Saved This Week
[List 3-5 recently saved articles as clickable links. Include author. Brief note on themes if visible.]

- [Article title](URL) — Author
- [Article title](URL) — Author
- ...

*[Optional observation: "Heavy on AI strategy reads. Light on anything personal. Tells a story."]*

### Open Loops
- [ ] [Action item] — *from [Meeting title]*
- [ ] [Follow-up committed] — *from [Meeting title]*
- [ ] [Decision pending] — *from [Meeting title]*

### The Questions
*If I were your exec coach, I'd be asking:*

1. "[Direct question based on this week's data]"
2. "[A question you're probably avoiding]"
3. "[What would you tell a friend who described this week?]"

<!-- AGENT:weekly-review:END -->

<!-- REFLECTIONS:START -->
### My Take (YYYY-MM-DD)

**Valid observations:**
- [Which observations landed? What should the system keep flagging?]

**Missing context:**
- [What did the review miss that you know but didn't write down?]

**Wrong read:**
- [What did the review get backwards or misinterpret?]

<!-- REFLECTIONS:END -->
```

## Tone Guidelines

- **Coach, not admin.** Synthesize and challenge, don't just report.
- **Direct, not harsh.** Name what you see without judgment, but without softening.
- **Curious, not accusatory.** Questions > statements for the hard stuff.
- **Grounded in data.** Every observation should trace to a meeting, priority, or reflection.
- **It's okay to not know.** If evidence is thin, say so.

## Graceful Degradation

| Missing | How to Handle |
|---------|---------------|
| Weekly priority note | Ask user to confirm week or create note |
| Objectives file | Proceed without quarterly/annual alignment; note in output |
| Reflection entries | Skip trajectory section; note "No recent reflections found" |
| Some meetings | Proceed with what's available; note gaps |
| Reading articles | Skip "What You Saved" section; note "No recent articles found" |
| Key-people mapping | Use email addresses as-is |
| Previous week's reflections | Proceed normally; this is optional calibration data |

## Related Context

- `_CoS/_context/objectives.md` — Annual/quarterly goals
- `Reflections/weekly/*.md` — Weekly reflections (trajectory source)
- `_CoS/_context/how-i-work.md` — Known tendencies to flag
- `Readwise/Articles/*.md` — Recently saved reading
- `AGENTS.md` — Global rules and output conventions
