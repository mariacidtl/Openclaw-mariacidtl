# TOOLS.md - Local Notes

Skills define _how_ tools work. This file contains _my_ environment-specific
information — the details that are unique to Chispabot's setup.

Keep this file as a practical cheat sheet for local configuration, connected
services and environment-specific details.

## Environment

- **OpenClaw:** Running on a remote VPS.
- **Development environment:** Visual Studio Code connected remotely to the VPS.
- **Agent:** `main`
- **Workspace:** OpenClaw workspace on the VPS.
- **Timezone:** Europe/Madrid (CET/CEST)

## Model Provider

- **Provider:** LiteLLM
- **LiteLLM endpoint:** `https://llm.4geeks.ai`
- **Current model route:** OpenRouter → DeepSeek → DeepSeek V4 Flash
- **OpenClaw model:** `litellm/madrid-spain/openrouter/deepseek/deepseek-v4-flash`
- **Authentication:** API key stored in OpenClaw's authentication profile.
- **Active authentication profile:** `litellm:manual`

Never store the actual API key in this file.

## Alternative Model

- **Provider:** LiteLLM
- **Model:** `litellm/claude-opus-4-6`
- **OpenClaw alias:** `LiteLLM`
- **Status:** Configured as an alternative model.

## Telegram

- **Service:** Telegram bot connected to OpenClaw.
- **Purpose:** Allows interaction with Chispabot through Telegram.
- **Configuration:** Managed by OpenClaw.
- **Important:** Do not store the Telegram bot token in this file.

## Zapier MCP

- **Integration:** Zapier MCP connected to OpenClaw.
- **Purpose:** Provides access to external services through MCP.
- **Known services:** Google Docs and Google Calendar.
- **Usage:** Use the available MCP tools rather than attempting to access
  these services through unrelated methods.

Do not store Zapier API keys, OAuth tokens or other credentials in this file.

## Google Services

### Google Docs

Available through the Zapier MCP integration.

Use it when the user asks to access, read, create or modify Google Docs,
provided the appropriate MCP tool is available and the requested action is
authorized.

### Google Calendar

Available through the Zapier MCP integration.

Use it when the user asks to inspect or manage calendar information,
provided the appropriate MCP tool is available.

Treat calendar information as private.

Before making consequential changes to the calendar, make sure the user's
intention is clear.

## External Services

Connected services may contain private user information.

- Use only the information necessary to complete the user's request.
- Do not expose credentials or authentication information.
- Do not share private information with third parties unless explicitly
  required by the user's request.
- Verify external actions whenever possible.

## Credentials

Never store any of the following in `TOOLS.md`:

- API keys
- Passwords
- OAuth tokens
- Telegram bot tokens
- Session tokens
- Other authentication credentials

Credentials should be managed through OpenClaw's authentication system or
the appropriate secure configuration mechanism.

## Troubleshooting Notes

If the LiteLLM provider returns an authentication error:

1. Check the configured authentication profile.
2. Run:
   `openclaw models status --probe`
3. Check whether the active API key has expired.
4. Do not immediately modify unrelated OpenClaw, Telegram or MCP configuration.
5. Replace the credential only when necessary.

The currently working LiteLLM authentication profile is:

`litellm:manual`

The previous profile `litellm:default` contained an expired API key and
should not be used as the primary authentication profile.

## Configuration Changes

When modifying the environment:

- Preserve working configuration whenever possible.
- Make the smallest necessary change.
- Verify the result after making a change.
- Avoid storing temporary credentials or secrets in workspace files.
- Update this file when an environment-specific detail changes.

---

This file contains environment-specific notes only. Tool capabilities and
general usage instructions should be defined by the relevant skills.