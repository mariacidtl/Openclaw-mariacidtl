---
name: learning-diary
description: "Transform informal learning notes into a structured entry and append to Google Docs learning diary via Zapier MCP."
allowed-tools:
  - mcporter
  - exec
user-invocable: true
---

# Learning Diary

Use when Maria wants to record what she learned in her AI Engineer studies. Takes informal notes, structures them, and appends to her Learning Diary document in Google Docs.

## Context

- Maria is studying an AI Engineer course (from `USER.md`)
- Google Docs is available through Zapier MCP (from `TOOLS.md`)
- Maria's timezone: Europe/Madrid
- Document ID: `1XODfjEfpt4gZkaYTOeS-GACKsK6J_h2eo3LruTutzxE`
- Document: "Learning Diary - AI Engineer"

## Input needed

Natural language description of what Maria learned. Example:

> Today I learned how OpenClaw skills work, how agent configuration files provide persistent context, and how LiteLLM authentication profiles work.

The input does not need to follow a fixed format.

## Workflow

1. Ask Maria what she learned today (if not already provided).
2. If the information is insufficient, ask for clarification.
3. Structure the entry with:
   - Date (current date in Europe/Madrid)
   - Main topics learned
   - Key concepts or lessons (with concrete details)
   - Useful observations
   - Optional questions or topics to explore later
4. Append the entry to the Google Docs document using Zapier MCP's `append` action.
5. Verify the operation succeeded from the API response.
6. Report to Maria that the entry was added with the document link.

## Entry format

```
### 📝 [Date]

**Temas principales:**
- [tema 1]
- [tema 2]

**¿Qué aprendí?**
[2-3 sentences describing what was learned]

**Conceptos clave:**
- [concepto 1]
- [concepto 2]

**Observaciones:**
[optional observations]

**Preguntas para explorar después:**
- [question 1]
- [question 2]
```

## Execution

Use `mcporter call` with `execute_zapier_write_action` for `GoogleDocsV2CLIAPI`, action `append`, tool_name `google_docs_append_text_to_document`, params:
- `file`: the document ID
- `text`: the formatted entry
- `newline`: `"false"`

## Constraints

- Never invent information
- Ask for clarification when input is genuinely ambiguous
- Use current date in Europe/Madrid timezone
- Keep entries concise and useful for later revision