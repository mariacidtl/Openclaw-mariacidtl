---
name: smart-calendar
description: "Convert natural-language descriptions of study sessions or appointments into structured Google Calendar events via Zapier MCP."
allowed-tools:
  - mcporter
  - exec
user-invocable: true
---

# Smart Calendar Events

Use when Maria wants to create a calendar event from a natural language description.

## Context

- Maria is studying an AI Engineer course (from `USER.md`)
- Google Calendar is available through Zapier MCP (from `TOOLS.md`)
- Maria's timezone: Europe/Madrid
- Default calendar: `mariaciddigitalmarketing@gmail.com`

## Input needed

Natural language description of the intended event. Example:

> I want to study OpenClaw for two hours on Thursday afternoon and get a reminder 30 minutes before.

## Workflow

1. Parse the description to extract:
   - **Title**
   - **Date** (resolve relative dates in Europe/Madrid)
   - **Start time** (resolve relative times)
   - **Duration** or **end time**
   - **Location** if mentioned
   - **Reminder** if requested
   - **Description** if useful
   - **Recurrence** if mentioned
2. If any required detail is ambiguous, ask for clarification.
3. Create the event using `Create Detailed Event` in Google Calendar.
4. Verify the operation succeeded (check for `status: "confirmed"`).
5. Confirm the event details to Maria.

## Execution

Use `mcporter call` with `execute_zapier_write_action` for `GoogleCalendarCLIAPI`, action `detailed_event`, tool_name `google_calendar_create_detailed_event`.

Required params:
- `calendarid`: `"mariaciddigitalmarketing@gmail.com"`
- `summary`: event title
- `start__dateTime`: ISO datetime in Europe/Madrid
- `end__dateTime`: ISO datetime in Europe/Madrid
- `eventType`: `"default"`
- `all_day`: `"no"` (unless all-day)
- `useCustomColor`: `"no"`
- `recurrence_frequency`: if recurring
- `recurrence_until`: if recurring, end date
- `transparency`: `"opaque"` (busy)
- `reminders__useDefault`: `"yes"`
- `visibility`: `"default"`
- `conferencing`: `"no"`
- `location`: if mentioned

## Constraints

- Never guess ambiguous dates or times without asking
- Always use Europe/Madrid timezone
- Confirm event details to Maria after creation