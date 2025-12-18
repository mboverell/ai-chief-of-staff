# AI Chief of Staff

A system for turning meeting notes, priorities, and reflections into executive-level insight—using AI agents and a structured Obsidian vault.

*Week 1 reflections. I'll revisit this at Week 4 to see what's changed.*

---

## The Problem

Seven years of notes about myself—annual objectives, weekly reflections, meeting summaries, reading highlights—scattered across Apple Notes, Google Docs, and various tools. Valuable context, completely fragmented.

I knew there were patterns I couldn't see. Commitments I'd forgotten. Contradictions between what I said I wanted and what I actually did. What I needed was something like an executive coach or Chief of Staff: someone to connect the dots, surface blind spots, and ask uncomfortable questions.

The obvious solution: dump everything into ChatGPT. But I wasn't willing to hand seven years of personal context—goals, vulnerabilities, relationship dynamics—to a system I don't fully trust, with no control over storage or usage.

**The question:** Could I build something different? A system that was modular and flexible. That didn't require giving up my privacy and context. That let me access the best of any model, at any time, without lock-in.

---

## The Design Constraints

Before building, I needed to solve for three concerns:

### Model Independence

I want to use different models for different purposes—Claude for nuanced analysis, GPT-4 for certain tasks, local models for sensitive content. The worst outcome would be years of context locked inside one vendor's memory system.

**The solution:** Own the context layer yourself. Store everything in plain text files. The intelligence is stateless; the memory lives in your file system. Swap models anytime without losing anything.

### Privacy

I'm not a privacy absolutist, but I trust some providers more than others. I'd rather maintain the ability to run intelligence across personal context that stays local to me.

Using models via API (through Cursor) rather than web interfaces means no persistent memory is built. My data isn't used for training. A meaningful side benefit.

### Portability

I was spending too much time copying content from chat interfaces into documents. Outputs felt ephemeral, trapped in threads I'd never find again.

This system inverts that. Everything exists as a Markdown file—inputs, outputs, context. The AI writes directly into my vault. Nothing locked in a chat window. All knowledge artifacts as plain text.

---

## The Approach

### Core Philosophy

**Make the implicit explicit.** The system works because I've documented:
- Who I am and what I'm trying to do (objectives, priorities)
- How I work and where I go wrong (patterns, tendencies)
- What actually happened (meetings, reflections)

The AI doesn't need to be brilliant. It needs to be thorough and honest. Most of the value comes from structured synthesis, not creative insight.

**Steel thread first.** One skill, working end-to-end, validated over 4+ weeks before expanding. Quality over breadth. This prevents the common failure mode of building infrastructure that never delivers value.

**Progressive disclosure.** The system loads context on demand, not all at once. Global rules are always available; specific context is loaded when relevant. This keeps the AI focused and the system maintainable.

---

## Architecture

### Visual Schema

```
┌─────────────────────────────────────────────────────────────────────────┐
│                            OBSIDIAN VAULT                               │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     SYSTEM LAYER (_CoS/)                         │   │
│  │                                                                  │   │
│  │   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │   │
│  │   │   CONTEXT    │  │    SKILLS    │  │    STATE     │          │   │
│  │   │              │  │              │  │              │          │   │
│  │   │ • objectives │  │ • weekly-    │  │ • open-      │          │   │
│  │   │ • how-i-work │  │   review     │  │   actions    │          │   │
│  │   │ • key-people │  │ • (future)   │  │ • (future)   │          │   │
│  │   │              │  │              │  │              │          │   │
│  │   └──────┬───────┘  └──────┬───────┘  └──────┬───────┘          │   │
│  │          │                 │                 │                   │   │
│  │          │    loaded by    │   reads/writes  │                   │   │
│  │          └────────────────►│◄────────────────┘                   │   │
│  │                            │                                     │   │
│  └────────────────────────────┼─────────────────────────────────────┘   │
│                               │                                         │
│                               │ analyzes                                │
│                               ▼                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     CONTENT LAYER                                │   │
│  │                                                                  │   │
│  │   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │   │
│  │   │  PRIORITIES  │  │   MEETINGS   │  │  REFLECTIONS │          │   │
│  │   │              │  │              │  │              │          │   │
│  │   │ Weekly plans │  │ Transcripts  │  │ Periodic     │          │   │
│  │   │ with Work +  │  │ with         │  │ self-        │          │   │
│  │   │ Personal     │  │ attendees,   │  │ reflection   │          │   │
│  │   │ split        │  │ timestamps   │  │ entries      │          │   │
│  │   │              │  │              │  │              │          │   │
│  │   └──────────────┘  └──────────────┘  └──────────────┘          │   │
│  │                                                                  │   │
│  │   ┌──────────────┐  ┌──────────────┐                            │   │
│  │   │   READING    │  │   PROJECTS   │                            │   │
│  │   │              │  │              │                            │   │
│  │   │ Saved        │  │ Working      │                            │   │
│  │   │ articles &   │  │ notes by     │                            │   │
│  │   │ highlights   │  │ project      │                            │   │
│  │   │              │  │              │                            │   │
│  │   └──────────────┘  └──────────────┘                            │   │
│  │                                                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘

                                    │
                                    │ AI agent reads vault
                                    │ via Cursor
                                    ▼

┌─────────────────────────────────────────────────────────────────────────┐
│                           AI AGENT (Cursor)                             │
│                                                                         │
│   1. Reads AGENTS.md for global rules + skill index                     │
│   2. Loads relevant SKILL.md based on task                              │
│   3. Gathers inputs from content layer                                  │
│   4. Produces output in replaceable blocks                              │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Key Components

| Component | What It Contains | Why It Matters |
|-----------|------------------|----------------|
| **AGENTS.md** | Identity, vault structure, skill index, output rules | The "system prompt" — what the AI always knows |
| **Skills** | Task-specific procedures with inputs, process, output format | Modular, testable units of work |
| **Context** | Objectives, working patterns, key people | Background loaded on demand |
| **State** | Persistent data the AI can update | Enables tracking across sessions |
| **Content** | Meetings, priorities, reflections, reading | The raw material for synthesis |

### Data Flow

```
Weekly Priority Note  ──┐
                        │
Meetings (this week) ───┼──► Weekly Review Skill ──► Appended Analysis Block
                        │
Reflections (recent) ───┤
                        │
Reading (saved) ────────┤
                        │
Context files ──────────┘
```

---

## Design Principles

### 1. Separation of Concerns

Each layer has a single responsibility:

- **AGENTS.md** — Global rules (always loaded)
- **Skills** — Task procedures (loaded on demand)
- **Context** — Background information (referenced by skills)
- **State** — Persistent data (agent-writable)
- **Content** — Human-generated raw material (append-only)

This separation makes the system maintainable. Adding a new skill doesn't require changing global config. Context can evolve independently.

### 2. Progressive Disclosure

Not everything is loaded at once:

- **Level 1:** AGENTS.md provides identity and skill index
- **Level 2:** Relevant skill loaded when task matches
- **Level 3:** Skill references specific context files

This keeps the AI focused and reduces token cost.

### 3. Constrained Output

The AI can only write to:
- **Replaceable blocks** within existing notes (using HTML comments as markers)
- **State files** in `_CoS/_state/` (can be fully overwritten)

Everything else is append-only. This prevents the AI from corrupting human content while enabling persistent tracking.

### 4. Citations Required

Every extracted insight must reference its source file. This builds trust through traceability. If the AI says "you committed to X," I can verify it.

### 5. Graceful Degradation

Skills handle missing inputs with warnings, not failures. The system works with incomplete context. This is critical—waiting for "complete" data means never starting.

---

## The Weekly Review Skill (V1)

The first and currently only skill. Designed to work like an executive coach, not an admin assistant.

### What It Does

Synthesizes a week's worth of meetings, priorities, reflections, and reading into:

1. **Snapshot** — Meeting count, open loops, priority progress, energy signal
2. **Trajectory** — Multi-week arc from reflections (not just this week in isolation)
3. **Narrative synthesis** — What's actually happening, in coach voice
4. **Reading patterns** — What I saved reveals what's on my mind
5. **Open loops** — Action items with sources
6. **The Questions** — What an elite coach would ask given this data

### What Makes It Different

**Coach, not admin.** The output isn't "you had 14 meetings." It's "the exploration sprint appears frozen while urgent requests consume bandwidth."

**Multi-week context.** Reads recent reflections to understand trajectory. Is this week an anomaly or part of a trend?

**Reading as signal.** What someone saves to read reveals priorities they haven't articulated. The skill surfaces this.

**Self-improving.** Each review ends with a reflections template. User feedback on what landed vs. missed calibrates future reviews.

---

## What's Working (Week 1 Observations)

*System is in Phase 1: Steel Thread validation. Check back at Week 4.*

**Infrastructure is solid.** Folder structure, file conventions, and skill procedure are stable. The agent finds files correctly.

**Progressive disclosure works.** Loading context on demand keeps reviews focused.

**Coach voice is valuable.** The shift from "reporting" to "observing" makes the output worth reading.

**Model independence feels real.** I've already switched between Claude models during development. The context is in files, not in any model's memory. If I wanted to try a different provider tomorrow, I could.

**Portability is liberating.** Every interaction produces artifacts I own. No more copying from chat windows.

**Open questions for Week 4:**
- Is the reading integration adding signal or noise?
- Does the reflections feedback loop actually improve output over time?
- How much trajectory context is optimal (2 weeks? 4 weeks?)
- Will I actually use the weekly review consistently, or will it decay?

---

## What's Next

| Phase | Focus | Status |
|-------|-------|--------|
| **Phase 1** | Validate weekly review over 4+ weeks | In progress |
| **Phase 2** | Add persistent action tracking | Planned |
| **Phase 3** | Meeting prep skill, monthly reflection | Future |

The discipline is to not build Phase 2 until Phase 1 is proven useful.

---

## Technical Notes

**Current Stack:**
- Obsidian vault (Markdown files with YAML frontmatter)
- Cursor IDE with AI agent (Claude)
- Meeting transcription tool → Markdown export
- Reading highlight sync → Obsidian

**Current Constraint:** The system works with files, not APIs. No direct access to email, Slack, or calendar—everything is inferred from file exports.

This is intentional *for now*. The file-first approach keeps things simple and portable while the core system is validated. But it's not the end state.

### What I'm Curious to Explore

**Calendar Integration**
The obvious next data source. Real calendar data would enable:
- True time allocation analysis (not just meeting counts)
- Proactive prep before meetings
- Pattern detection across weeks (when do I have deep work time?)

The question: pull via API, or find an export-to-markdown flow that preserves the file-first philosophy?

**Personal CRM**
I have relationship notes, but they're sparse. A lightweight CRM layer could track:
- Last interaction with key people
- Follow-up commitments
- Relationship health signals

This could be a skill that synthesizes from meetings + calendar, or a separate state file the agent maintains.

**Email Integration (Agentic)**
The more ambitious frontier. Not just reading email for context, but agents that can:
- Draft responses based on my patterns and priorities
- Flag items that need attention
- Take action on my behalf (with approval gates)

This requires higher trust in the system and more sophisticated guardrails. Probably Phase 4+ territory, but it's the direction that gets genuinely interesting.

---

## Repo Structure

```
ai-chief-of-staff/
├── README.md                    # This file — the story and philosophy
├── CHANGELOG.md                 # Evolution notes (Week 1, Week 4, etc.)
│
├── system/                      # The shareable architecture
│   ├── AGENTS.md                # Template system prompt
│   ├── SCHEMA.md                # How to structure your vault
│   └── skills/
│       └── weekly-review/
│           └── SKILL.md         # The skill procedure
│
├── templates/                   # Blank starter templates
│   ├── objectives.md
│   ├── how-i-work.md
│   ├── key-people.md
│   └── weekly-priorities.md
│
└── examples/                    # Fictional populated examples
    └── weekly-review-output.md
```

---

## Lessons Learned

1. **Start with the output you want.** Design the weekly review output format first, then work backwards to what inputs you need.

2. **Existing content structures matter.** Adapting to your meeting tool's frontmatter is easier than changing export settings.

3. **The AI needs constraints, not freedom.** Telling it exactly where to write and what format to use produces better results than open-ended instructions.

4. **Coach voice > Admin voice.** The early versions that "reported" meetings were useless. The versions that "observed patterns" are valuable.

5. **One skill is enough to start.** The temptation to build multiple skills at once is strong. Resist it.

6. **Own the context layer.** The key insight: put all memory and context in plain text files you control. Models are stateless executors. This gives you model independence, portability, and privacy in one move.

7. **Text files are underrated.** Markdown is universal, version-controllable, searchable, and readable by any LLM. Treating all knowledge artifacts as plain text simplifies everything.

---

## Feedback & Discussion

This is a work in progress. I'm sharing it to learn in public and connect with others thinking about similar problems.

**If you have questions, ideas, or want to share how you've adapted this:**
- Open a thread in [Discussions](https://github.com/mboverell/ai-chief-of-staff/discussions)
- Or open an [Issue](https://github.com/mboverell/ai-chief-of-staff/issues) for specific feedback

I'm particularly interested in:
- How others are approaching AI + personal knowledge management
- Feedback and ideas for extending the system
- What's missing or confusing in the documentation
- Alternative architectures or design choices

---

## License

MIT License — use freely, adapt as needed.

---

*Week 1: December 2024*
