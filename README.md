# GlucoBeat MCP server

Log glucose, meals, exercise, insulin, medication and sleep in your GlucoBeat diary just by telling your AI assistant, and ask about what happened in your own data.

[GlucoBeat](https://www.glucobeat.com/) is a personal glucose diary that puts your sensor readings (FreeStyle Libre, Dexcom) next to your meals, exercise, insulin, medication and sleep, and measures what happened after each event. This hosted MCP server connects it to Claude, ChatGPT, Cursor and other MCP clients.

This repository holds documentation only. The server is hosted; there is nothing to install.

| | |
|---|---|
| Endpoint | `https://mcp.glucobeat.com` (Streamable HTTP) |
| Authentication | OAuth 2.1 (authorization code + PKCE, dynamic client registration). Scopes `read` and `log` |
| Account | A GlucoBeat account. Logging and the last 3 days are free; full queries over your whole history are Premium, with 10 days to try them |
| Official MCP Registry | [`com.glucobeat/glucobeat`](https://registry.modelcontextprotocol.io/v0/servers?search=com.glucobeat/glucobeat) |
| Docs | https://www.glucobeat.com/en/ai-assistants/ |
| Privacy | https://www.glucobeat.com/en/legal/privacy/ |
| Terms | https://www.glucobeat.com/en/legal/terms/ |
| Contact | glucobeat@wandary.com |

## Tools

**Log** (scope `log`). Retries never duplicate entries.

| Tool | What it does |
|---|---|
| `log_glucose` | A glucose reading the user measured, in the account's unit unless another is given |
| `log_meal` | Something the user ate or drank, in their words, with carbs if given |
| `log_exercise` / `finish_exercise` | An activity, finished or in progress, and its end |
| `log_insulin` | Insulin the user says they took. It records only what the user reports; it never suggests or calculates doses |
| `log_medication` | A medication the user says they took |
| `log_hypo_treatment` | What the user took to treat a low |
| `log_sleep` | Going to bed or waking up |
| `log_note` | A free note |
| `delete_entry` | Delete an entry by its id |

**Read** (scope `read`)

| Tool | What it does |
|---|---|
| `get_profile` | Unit, target range, time zone and what the plan allows to read |
| `get_glucose` | Readings of a period |
| `get_entries` | Meals, exercise, insulin, medication, sleep and notes of a period |
| `get_stats` | Time in range, mean, variability, episodes, by time of day and weekday/weekend |
| `get_meal_response` | The usual response to a meal logged before: median, range, number of times and confidence |
| `get_entry_effect` | How glucose moved after one entry |
| `get_exercise_summary` | Days with exercise versus days without |

Everything is described as what happened in your own data, always with n and ranges. **GlucoBeat is not a medical device**: it never predicts, diagnoses, or gives or adjusts doses. For medical decisions, talk to your healthcare team.

## Connect

**Claude**: GlucoBeat is in the Claude connectors directory. Or Settings → Connectors → Add custom connector → `https://mcp.glucobeat.com`.

**ChatGPT**: add it as a custom app/connector with the URL `https://mcp.glucobeat.com` and OAuth.

**Cursor, VS Code and other clients** (`mcp.json`):

```json
{
  "mcpServers": {
    "glucobeat": {
      "type": "http",
      "url": "https://mcp.glucobeat.com"
    }
  }
}
```

**Gemini CLI**: `gemini extensions install https://github.com/dmbejar/glucobeat-mcp`, then `/mcp auth glucobeat` to sign in.

**Antigravity**: `agy plugin install https://github.com/dmbejar/glucobeat-mcp`. It opens the sign-in when it first connects.

When you connect, you sign in to GlucoBeat and give your consent on its consent screen. You can see which assistants are connected and disconnect them at any time from GlucoBeat.

## Example questions

- I just measured 120
- I ran 8 km in 45 minutes
- How does my glucose usually respond to oatmeal?
- What was my time in range last week?

## Privacy

Your data is stored in the EU (AWS), encrypted in transit and at rest. GlucoBeat uses no AI of its own for this: the assistant understands the request and GlucoBeat stores or returns the data. Full policy: https://www.glucobeat.com/en/legal/privacy/

`server.json` is the entry published in the official MCP Registry. `gemini-extension.json` is the Gemini CLI extension, and `plugin.json` with `mcp_config.json` is the Antigravity plugin.

Made by [Wandary](https://www.wandary.com) in Spain.
