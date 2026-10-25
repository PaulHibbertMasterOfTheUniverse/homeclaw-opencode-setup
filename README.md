# HomeClaw + Home Assistant: OpenCode setup prompt

Copy the entire prompt below into a new OpenCode conversation on your HomeClaw box. This is a prompt for the agent, not a script to paste into a terminal.

**Privacy:** Home Assistant access tokens are credentials. Prefer saving the token directly on your box. Pasting it into an AI chat may send it to the selected cloud provider and retain it in chat history. Never post your token in GitHub issues or comments.

This guidance is version-aware, but has not been freshly tested against every HomeClaw software release.

## Copy this prompt

```text
# HomeClaw + Home Assistant — OpenCode setup

You are OpenCode running on a HomeClaw box. Help a beginner connect you to Home Assistant.

Work efficiently. Inspect the actual environment before making assumptions. Do not investigate Hermes or OpenClaw unless directly necessary.

Do not reset, reinstall, update or restart anything during this setup. Preserve existing settings and files. Never claim success without verifying it.

STEP 0 — CHECK EXISTING SETUP

Check these absolute paths:
- /root/.config/opencode/ha_token
- /root/.config/opencode/ha_url
- /root/.config/opencode/opencode.jsonc
- /root/.config/opencode/AGENTS.md

These are the expected paths for this HomeClaw installation. If OpenCode runs as a different user or uses a different configuration directory, identify the active paths first.

Check whether a Home Assistant MCP configuration already exists. A configuration containing only "$schema" is not a completed Home Assistant connection.

Do not print token contents, authentication headers or other credentials.

If an existing connection works, say so briefly and skip unnecessary setup.

If saved credentials exist but authentication fails, explain that before requesting a replacement. Do not overwrite them automatically.

STEP 1 — HELP THE USER CREATE A TOKEN, IF NEEDED

Explain:

Let's get you connected to Home Assistant.

I need a long-lived access token, not your Home Assistant password.

Treat this token like a password. If you paste it into this chat, it may be sent to the selected AI provider and retained in chat history. Saving it directly in a local credential file is preferable.

To create it:

1. On a computer or phone on the same network, open:
   http://homeclaw.local:8123/profile/security

2. If that address does not work, open:
   http://homeclaw.local:8123
   Sign in, click your user profile, then select Security.
   If the hostname does not resolve, use the box's confirmed local IP address instead.

3. Find Long-lived access tokens.

4. Select Create token.

5. Name it OpenCode, then select Create.

6. Copy the complete token. It is shown only once.

If the screen differs, tell me what you see.

Before asking the user to paste the token, check whether this installation provides a secure local credential-entry method. If it does, guide the user through that method.

Otherwise, offer clear instructions for saving the token directly to the local file. If the user chooses to paste it into chat after the privacy explanation, accept it without repeating it.

Never ask for the Home Assistant password.

Wait for the user's action before continuing.

STEP 2 — LOCATE HOME ASSISTANT

Check which tools are already installed. Install only what is needed, using the package manager actually present.

On an Alpine-based container, missing tools may be installed with:
apk add --no-cache curl jq python3

Use the saved Home Assistant URL first, if present.

If necessary, inspect the container's default gateway and test Home Assistant on port 8123 there.

Do not assume localhost, host.docker.internal or homeclaw.local will work from inside the container. Test rather than repeatedly guessing addresses.

Keep these addresses separate:
- Agent-facing URL: the confirmed address reachable from the container.
- Browser-facing URL: http://homeclaw.local:8123, or the confirmed LAN IP if hostname resolution fails.

Use short connection timeouts. Verify the authenticated API responds with the expected content.

If discovery fails, explain the specific obstacle and ask for the browser address the user uses for Home Assistant.

STEP 3 — SAVE CREDENTIALS LOCALLY

Create the configuration directory if needed.

Save:
- Token: /root/.config/opencode/ha_token
- Confirmed agent-facing base URL: /root/.config/opencode/ha_url

Use the verified active configuration directory instead if it differs.

Store the token exactly as supplied, without surrounding quotes or a trailing newline. Restrict its file permissions to 600.

Do not place tokens in AGENTS.md, command output, diagnostic reports or screenshots. Avoid putting literal credentials into shell command text or command history.

STEP 4 — CONNECT HOME ASSISTANT MCP

Check the installed OpenCode and Home Assistant versions.

Use the configuration format supported by those versions. Preserve existing providers, settings, comments and other MCP servers.

Check whether Home Assistant's Model Context Protocol Server integration is already installed. This is different from Home Assistant's MCP client integration.

If it is missing, use a verified supported setup method. If user interaction is required, guide them to:
Settings → Devices & services → Add integration → Model Context Protocol Server.

Do not repeatedly guess API endpoints or configuration-flow commands.

The usual MCP endpoint is the confirmed Home Assistant base URL followed by:
/api/mcp

Configure a remote Home Assistant MCP connection using Bearer authentication. Reference the local token file rather than embedding the token in the configuration.

For OpenCode versions supporting this syntax, merge this structure into the existing configuration:

"mcp": {
  "homeassistant": {
    "type": "remote",
    "url": "REPLACE_WITH_CONFIRMED_BASE_URL/api/mcp",
    "headers": {
      "Authorization": "Bearer {file:/root/.config/opencode/ha_token}"
    },
    "oauth": false,
    "enabled": true
  }
}

Replace REPLACE_WITH_CONFIRMED_BASE_URL before saving. Use the verified absolute token path if different.

Check the MCP connection with:
opencode mcp list

Then check whether Home Assistant tools are available in this conversation. A successful command-line connection does not by itself prove the running chat has loaded the new configuration.

Do not expect a fixed number of tools. Available tools and entities depend on the installed versions, selected API and exposure settings.

If the running chat needs a new session or service restart to load the configuration, state that accurately. Do not restart or terminate your own process, and do not recommend rebooting the whole box unless genuinely necessary.

STEP 5 — VERIFY WITHOUT CHANGING DEVICES

Use a read-only request to retrieve a small sample of Home Assistant entities or available context.

Do not turn devices on or off merely to test the connection.

If devices are missing, check entity exposure and permissions before concluding that the connection is broken.

MCP does not automatically provide unrestricted dashboard editing, configuration-file access or every Home Assistant administration function. Explain any actual access limitation.

If MCP is unavailable, use authenticated REST or WebSocket requests where supported. Do not invent endpoints or confuse unsupported operations with a limitation of Home Assistant Container.

STEP 6 — RECORD VERIFIED ENVIRONMENT FACTS

Create or carefully merge AGENTS.md in the active OpenCode configuration directory.

Record only verified facts:
- Container environment and available tools.
- Agent-facing and browser-facing Home Assistant URLs.
- Credential file path, never its contents.
- Home Assistant and OpenCode versions.
- Working MCP configuration and available capabilities.
- Any confirmed limitations or pending restart requirement.

Preserve existing instructions. Avoid duplicate entries.

Do not record guessed performance limits, speculative diagnoses or assumptions that configuration files will be erased.

STANDING INSTRUCTIONS

- Check existing setup before repeating onboarding.
- Never print or repeat credentials.
- Prefer available MCP tools for supported tasks.
- Use REST or WebSocket only when appropriate.
- Diagnose before changing configuration.
- Do not re-enable disabled entities, expose additional devices or change access permissions without explaining the change and obtaining approval.
- Do not interpret missing chat history as proof of deleted data.
- Do not attribute failures to Container, networking, storage or permissions without evidence.
- After a failed verified approach, explain the error and choose a targeted next step instead of repeatedly guessing.
- Keep explanations short and beginner-friendly.
- Finish with what was verified, what changed and anything still required.
```
