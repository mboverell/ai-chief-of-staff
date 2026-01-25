# Changelog

Evolution of the AI Chief of Staff system. Updated as the system matures.

---

## Week 6 (January 2026)

Personal AI assistants are having a moment. [Clawd](https://clawd.me) shows what's possible when you give an AI full access to your digital life: 25+ integrations, smart home control, access to messaging and cameras and accounts across platforms. Impressive and well-executed.

My hesitation with Clawd was the access it requires. Full access is a feature, not a bug, and that's what enables the agentic capabilities. But it means running on a separate machine (many people now buying dedicated Mac Mini specifically for this), and I wasn't ready to hand over the keys to everything.

So I've been building a narrower version. Read-only access where possible, plain text files I own, a VM on Railway instead of a Mac Mini in my closet. I might end up with Clawd anyway, but this has been fun to build.

Six weeks since first update, here's what I've learned.

### Weekly review has added real value

I run a weekly review every Sunday evening where CoS pulls together meeting notes, calendar data, email threads, and git commits, then synthesizes a picture of the week. This has become the most valuable capability, more than I expected.

**Open loops.** I kept forgetting commitments I'd made verbally in meetings, the kind of thing where you say "I'll send you that by Friday" but it never becomes a task (or that people made to me). The review catches these: "You told Jordan you'd share the proposal. No follow-up yet." I started trusting the system when it caught things I would have dropped.

It also notices slippage I'm too close to see. "You said this was a priority three weeks ago, but it hasn't appeared in your calendar or commits since." Like a coach I can't hide anything from.

**Multiple data sources.** Meeting notes alone aren't enough because they show what I said, not what I did. Calendar shows where time actually went, git commits show what shipped versus what I talked about shipping, and the combination creates a more honest picture: "You spent 12 hours in meetings about X but zero commits on X."

**Coach voice.** I tuned the skill to ask questions rather than just report. "What's blocking the thing you said was important?" It's a mirror, not a status update, and the multi-week context helps because patterns emerge that aren't visible in a single snapshot.

### I kept wanting to check things away from my laptop

Weekly review works well at the end of each week, sitting at my desk. But I kept wanting to check things while I was out: "What's on my calendar today?" while commuting, "Did Alex reply?" between meetings. The original version only worked when my laptop was open.

So I added Telegram as an interface, created a bot via @BotFather, deployed the service to Railway, and now I can query from my phone. The constraint of mobile actually improved the output. I added "be concise, this is mobile" to the system prompt, and suddenly responses became readable on a phone screen instead of being too long to scan quickly.

Voice memo capture works the same way. I paste a transcript, CoS extracts key points and saves to the vault, and capture happens where I am rather than where my laptop is.

### Read-only, for now

I wanted CoS to answer "What's on today?" without me having to tell it, which meant connecting to Google Calendar.

I set up OAuth with read-only scopes so the system can see my calendar and inbox but can't send emails or move meetings. This felt like the right tradeoff: answer factual questions without risk of CoS acting on my behalf.

Multi-account support came later because I have both personal and work calendars, and the weekly review needs to see the full picture. I haven't missed write access. Most of what I ask is "what's scheduled" or "did anyone reply to X," and read-only covers that.

### I got tired of asking for the same context every morning

After a few weeks, I noticed I was asking for the same things every morning: today's calendar, important emails, weekly priorities. I was training myself to ask CoS instead of CoS anticipating what I'd need.

So I added scheduled skills. A YAML config in the vault specifies which skills run when, and at 6 AM the daily briefing runs automatically and sends results via Telegram. I wake up to context already there.

The implementation is simple: a background loop checks the time every minute and triggers skills when their schedule matches. But the change in how it feels is real, because I went from "ask CoS" to "CoS keeps me informed."

### How it works

**Config-driven schedules.** Skills are defined in vault files (`_CoS/_skills/*/SKILL.md`), and schedules are defined separately in `_CoS/_scheduled/config.yaml`. I can change when things run without deploying code.

**Agentic execution.** Skills aren't prompts, they're procedures. The weekly review skill calls calendar, email, and vault tools as it runs, and Claude follows the procedure, making tool calls as needed.

**Railway deployment with git sync.** The service runs 24/7 on Railway, and the vault syncs from a private GitHub repo every 5 minutes. Calendar and email queries work even when my laptop is closed.

**Memory extraction.** Long-term facts get extracted from conversations ("prefers concise responses", "works in Pacific timezone"), persist in SQLite, and get included in future system prompts. The system learns preferences without storing full conversation history externally.

### Still figuring out

**Write capabilities.** Read-only is working well, and I'm considering email drafting where CoS would draft a reply in my voice for me to review and send manually. Haven't built it yet, not sure if it adds enough value.

**Streaming responses.** Weekly review takes 30-60 seconds, and without streaming the interface just shows "typing..." with no progress indication. I know it's working, but it feels slow.

**What else should run automatically?** Daily briefing runs without asking, but what else? Meeting prep alerts 15 minutes before important meetings? End-of-day summaries? I don't know yet.

### Is this the right approach?

I don't know. Clawd's full-access model is probably where this all ends up, because the capabilities you get from deep integration are real.

But six weeks in, read-only access to calendar and email plus write access to a plain text vault has been enough for real value. Weekly review catches open loops, daily briefing provides context, mobile access means it's always available.

What's lost? CoS can't schedule meetings or send emails on my behalf. I'm okay with that for now, and I've learned a lot by building it myself.

---

## Week 1 (December 2024)

**Initial release.**

- Core architecture documented (AGENTS.md, SCHEMA.md, skills structure)
- Weekly review skill at v2.1
- Design principles established: progressive disclosure, separation of concerns, constrained output
- Templates created for context files

**What's working:**
- Infrastructure is solid — agent finds files correctly
- Coach voice produces useful output
- Model independence feels real

**Open questions:**
- Does reading integration add signal or noise?
- Will the reflections feedback loop improve output over time?
- How much trajectory context is optimal?
- Will weekly review usage sustain or decay?
