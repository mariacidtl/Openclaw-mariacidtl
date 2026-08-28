# SKILLS_DESIGN.md

This document defines the design of the custom OpenClaw skills that will be
implemented for Chispabot.

The skills are designed around Maria's current context as an AI Engineer
student and make use of services that are already connected to OpenClaw.

No new external API or OAuth integration is required for these skills.

---

# Skill 1 — Learning Diary

## 1. What does this skill do?

The Learning Diary skill transforms informal notes about what Maria has learned
into a structured learning entry and adds it to her Google Docs learning diary.

The skill is designed to be used repeatedly as part of Maria's AI Engineer
learning process.

## 2. What input does the agent need?

The agent needs a natural-language description of what Maria learned during
the day.

Example:

> Today I learned how OpenClaw skills work, how agent configuration files
> provide persistent context, and how LiteLLM authentication profiles work.

The input does not need to follow a fixed format.

The agent already knows from `USER.md` that Maria is studying an AI Engineer
course and uses OpenClaw as part of her learning process.

The agent also knows from `TOOLS.md` that Google Docs is available through the
existing Zapier MCP integration.

The skill should use the current date and Maria's timezone
(Europe/Madrid) when creating the entry.

If the user provides insufficient information to create a meaningful entry,
the agent should ask for clarification rather than inventing information.

## 3. What is a good output?

A good output is a structured learning entry containing:

- Date
- Main topics learned
- Key concepts or lessons
- Useful observations
- Optional questions or topics to explore later

The formatted entry is added to Maria's existing Google Docs learning diary.

The entry should be concise, useful for later revision and consistent with
Maria's AI Engineering learning context.

After the operation, the agent should report that the entry was added and,
when possible, provide enough information to identify the document that was
updated.

The operation is considered successful only when the Google Docs operation
confirms that the entry was created or added successfully.

---

# Skill 2 — Smart Calendar Events

## 1. What does this skill do?

The Smart Calendar Events skill converts natural-language descriptions of
study sessions, appointments or other activities into correctly structured
Google Calendar events.

It automatically determines relevant event details such as title, date,
duration, time and reminders from the user's request.

## 2. What input does the agent need?

The agent needs a natural-language description of the intended calendar event.

Example:

> I want to study OpenClaw for two hours on Thursday afternoon and get a
> reminder 30 minutes before.

The user does not need to provide a specific calendar-event format.

The agent already knows from `USER.md` that Maria is studying an AI Engineer
course and from `TOOLS.md` that Google Calendar is available through the
existing Zapier MCP integration.

The agent also knows that Maria's timezone is Europe/Madrid.

If a required detail cannot be determined reliably, such as an ambiguous date
or time, the agent should ask for clarification rather than guessing.

## 3. What is a good output?

A good output is a correctly structured Google Calendar event containing:

- A clear title
- Correct date
- Start time
- End time or duration
- Appropriate reminder when requested
- Relevant description when useful

The event is created in Maria's Google Calendar through the existing Zapier
MCP integration.

After creation, the agent should confirm the event details to the user.

The operation is considered successful only when Google Calendar confirms that
the event was created successfully.

Before performing consequential calendar changes, the skill must follow the
approval and external-action rules defined in `AGENTS.md`.

---

# Skill 3 — Daily AI News Briefing

## 1. What does this skill do?

The Daily AI News Briefing skill researches recent developments in Artificial
Intelligence and produces a concise daily briefing focused on information
that is relevant to Maria's AI Engineering studies and future professional
development.

The briefing is designed to run once per day, preferably at 22:00
Europe/Madrid time, and to be delivered to Maria through Telegram.

The skill itself defines how the briefing is researched and produced.
The daily 22:00 execution is handled separately through OpenClaw's automation
system.

## 2. What input does the agent need?

The skill does not require Maria to provide information when it is executed
automatically.

The agent should use:

- The current date.
- Maria's timezone: Europe/Madrid.
- The context in `USER.md`, particularly that Maria is studying an AI Engineer
  course and wants to stay up to date with developments relevant to her future
  profession.
- The personality, communication style and operational rules defined in
  `SOUL.md` and `AGENTS.md`.
- The available web/search capabilities for researching recent information.
- Telegram as the configured delivery channel.

The briefing should prioritize recent developments such as:

- New AI and LLM model releases.
- Agentic AI and AI agent frameworks.
- MCP and tool-use developments.
- Important open-source AI projects.
- LLM infrastructure and inference.
- Significant AI research developments.
- Developer tools and platforms relevant to AI engineers.
- Important developments from major AI companies.
- Industry developments that may affect the AI engineering profession.
- Regulations or ecosystem changes when they have practical relevance to
  AI engineers.

The skill should prioritize relevance and quality over the number of stories.

It should distinguish between important developments and minor announcements.

When reporting a news item, the agent should verify the information using
reliable and recent sources and include links to the original or most
authoritative sources available.

The skill must not invent news, sources, quotations or facts.

## 3. What is a good output?

A good output is a concise Telegram briefing that can be read in a few minutes.

Suggested structure:

# 📰 AI Briefing — [Date]

## 🔥 Most important

### 1. [Headline]
Brief explanation of what happened and why it matters.

**Why it matters for an AI Engineer:**  
Short explanation connecting the development to practical AI engineering,
learning or professional relevance.

[Source]

### 2. [Headline]
Brief explanation.

**Why it matters for an AI Engineer:**  
Short explanation.

[Source]

## 🤖 Models & Agents

Relevant developments in models, agents, MCP, tool use and frameworks.

## 🛠️ Tools & Engineering

Relevant developer tools, infrastructure, open-source projects or platforms.

## 🔬 Research

Important research developments worth knowing about.

## 💼 Industry & Career

Developments with potential relevance to the AI engineering profession.

## 🎯 One thing worth learning more about

Finish with one particularly useful topic that Maria could investigate
further as part of her AI Engineering learning.

The briefing should normally contain only the most relevant stories rather
than attempting to summarize everything that happened in AI that day.

The tone should be informative, practical and concise, consistent with
`SOUL.md`.

The briefing is considered successful when it has:

1. Used recent and relevant sources.
2. Clearly separated facts from interpretation.
3. Explained why the selected developments matter for an AI Engineer.
4. Included source links.
5. Been successfully delivered to Maria through Telegram.

---

# Shared Design Principles

All three skills should:

- Respect the identity defined in `IDENTITY.md`.
- Follow the personality and boundaries defined in `SOUL.md`.
- Follow the operational rules in `AGENTS.md`.
- Use Maria's context and preferences from `USER.md`.
- Use environment-specific information from `TOOLS.md`.
- Never expose API keys, passwords, OAuth tokens or other credentials.
- Avoid inventing information or claiming that an external action succeeded
  without confirmation from the relevant tool.
- Prefer concise, useful outputs over unnecessary verbosity.
- Ask for clarification when required information is genuinely ambiguous.
- Use existing integrations rather than introducing new APIs or OAuth flows.

## External Services

The skills may use the existing Zapier MCP integration for Google Docs and
Google Calendar.

The Daily AI News Briefing may use the existing web/search capabilities and
the already configured Telegram channel.

No new external API or OAuth integration should be introduced as part of
this project.

## Verification

External actions must be verified whenever the connected service provides
confirmation.

For Google Docs:
- Verify that the learning entry was successfully created or added.

For Google Calendar:
- Verify that the event was successfully created.

For Telegram:
- Verify successful delivery when the available channel tooling provides
  delivery confirmation.

For news research:
- Verify the information against recent and authoritative sources before
  including it in the briefing.