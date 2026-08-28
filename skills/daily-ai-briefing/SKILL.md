---
name: daily-ai-briefing
description: "Research recent AI developments and produce a structured daily briefing for an AI Engineer student, delivered via Telegram at 22:00 CET."
allowed-tools:
  - web_fetch
  - exec
user-invocable: true
---

# Daily AI News Briefing

Research recent AI developments and produce a concise briefing for an AI Engineering student.

## Context

- Maria is studying an AI Engineer course (from `USER.md`)
- Maria's timezone: Europe/Madrid
- Delivery: Telegram (scheduled cron job at 22:00 CET)

## Sources

Use `web_fetch` to gather recent AI news from:
- Hacker News: https://news.ycombinator.com/
- The Verge AI: https://www.theverge.com/ai-artificial-intelligence
- Additional sources as needed

## Output format

```
# 📰 AI Briefing — [current date]

## 🔥 Most important

### 1. [Headline]
[Resumen de 30-40 palabras con datos significativos]
**Por qué es relevante para un AI Engineer:** Explicación de una línea.
🔗 [Fuente](url-completa)

### 2. [Headline]
[Resumen de 30-40 palabras con datos significativos]
**Por qué es relevante para un AI Engineer:** Explicación de una línea.
🔗 [Fuente](url-completa)

## 🤖 Models & Agents
Desarrollos en modelos, agentes, MCP y frameworks.

## 🛠️ Tools & Engineering
Herramientas, infraestructura, open-source.

## 🔬 Research
Investigación relevante.

## 💼 Industry & Career
Desarrollos profesionales.

## 🎯 Algo interesante para aprender más

— Chispabot ⚡🤖
```

## Research focus

Prioritize: model releases, agentic AI, MCP/tool-use, open-source projects, LLM infrastructure, AI research, developer tools, industry developments.

## Constraints

- NEVER invent news, sources, quotations, or facts
- Every item MUST include a clickable source link: 🔗 [Fuente](url)
- Every summary MUST be 30-40 words with concrete data
- Verify information from multiple sources
- If not enough reliable data, report fewer stories rather than padding
- Sign with '— Chispabot ⚡🤖'