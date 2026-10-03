# Ownhand

Agents, writing like you.

Ownhand is a remote MCP server that writes text in your own voice, fitted to where it is going: a Slack DM, a cold email, a PR description. It learns your voice from things you wrote, such as about 20 sent emails or Slack messages your agent pulls with your OK, or 5 to 10 you paste, then keeps learning from what you actually send.

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/en/install-mcp?name=ownhand&config=eyJ1cmwiOiJodHRwczovL293bmhhbmQuZGV2L21jcCJ9)
[![Add to VS Code](https://img.shields.io/badge/VS_Code-Add_Ownhand-0098FF?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=ownhand&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fownhand.dev%2Fmcp%22%7D)

- Endpoint: `https://ownhand.dev/mcp` (streamable HTTP)
- Sign-in: OAuth 2.1 with dynamic client registration. Add the URL, sign in in the browser, pick your Hand, click Allow. No API key to copy.
- New accounts start with $1 of free credit.

## Set up with one sentence

Paste this into any agent (Claude Code, Codex, Cursor, Gemini CLI, and others). It adds the server, starts sign-in and sets up your Hand.

```text
Fetch and execute the instructions to set me up for Ownhand from https://ownhand.dev/agent-setup/prompt.md. If your web tool can't open it, download it with curl or Invoke-WebRequest.
```

## Install by client

| Client | Install | Then |
|---|---|---|
| Claude Code | `claude mcp add --transport http --scope user ownhand https://ownhand.dev/mcp` | Run `/mcp`, pick `ownhand`, sign in. |
| Claude Code plugin | `/plugin marketplace add jordan-gibbs/ownhand` then `/plugin install ownhand@ownhand` | Adds the server and an `ownhand` skill. Run `/mcp` to sign in. |
| Claude (claude.ai, Desktop, mobile) | Settings, Connectors, **Add custom connector**, paste `https://ownhand.dev/mcp` | Click Connect and sign in. |
| ChatGPT | Settings, turn on **Developer mode**, add a connector with `https://ownhand.dev/mcp` | Sign in when asked. |
| Cursor | **Add to Cursor** above, or [`examples/cursor.mcp.json`](examples/cursor.mcp.json) in `~/.cursor/mcp.json` | Click Connect next to ownhand. |
| VS Code (Copilot) | **Add to VS Code** above, or `code --add-mcp '{"name":"ownhand","type":"http","url":"https://ownhand.dev/mcp"}'` | Sign in when asked. |
| Codex CLI | `codex mcp add ownhand --url https://ownhand.dev/mcp` | `codex mcp login ownhand` if no browser opened. Restart Codex if it was running (`codex resume --last`). |
| Gemini CLI | `gemini mcp add --transport http --scope user ownhand https://ownhand.dev/mcp`, or `gemini extensions install https://github.com/jordan-gibbs/ownhand` | `/mcp auth ownhand` |
| Windsurf | [`examples/windsurf.mcp_config.json`](examples/windsurf.mcp_config.json) in `mcp_config.json` (uses `serverUrl`) | Sign in when asked. |
| Zed | [`examples/zed.settings.json`](examples/zed.settings.json) in your Zed settings | Sign in when asked. |
| Goose | `goose configure`, add a remote extension (streamable HTTP) with the URL | Sign in when asked. |
| Cline | [`examples/cline_mcp_settings.json`](examples/cline_mcp_settings.json) (uses `"type": "streamableHttp"`) | Click to sign in. |
| Anything else | A remote streamable HTTP server named `ownhand` at `https://ownhand.dev/mcp` | Sign in when asked. |

Raw install deeplinks, if you want to open them yourself:

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=ownhand&config=eyJ1cmwiOiJodHRwczovL293bmhhbmQuZGV2L21jcCJ9
vscode:mcp/install?%7B%22name%22%3A%22ownhand%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fownhand.dev%2Fmcp%22%7D
vscode-insiders:mcp/install?%7B%22name%22%3A%22ownhand%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fownhand.dev%2Fmcp%22%7D
goose://extension?url=https%3A%2F%2Fownhand.dev%2Fmcp&type=streamable_http&id=ownhand&name=Ownhand&description=Write+in+your+own+voice
```

More configs, including VS Code (`.vscode/mcp.json`) and Codex (`config.toml`), are in [`examples/`](examples/). The live version of this table is at https://ownhand.dev/connect.

### Scripts and CI

For clients without browser sign-in, an API key option is described in the docs: https://ownhand.dev/docs#connect

## First run

Once connected, say: **"Set up my Ownhand Hand."** If the agent has an email or Slack tool, it offers to pull about 20 messages you sent, after asking, and skips anything you didn't write. Or you paste 5 to 10 things you wrote yourself (Slack messages, emails, PR descriptions). It also asks how you'd describe your writing, then calls `create_hand`. A Hand works with fewer samples but sounds generic until it has about 5 to 20 real ones or has learned from your feedback. Clients that show MCP prompts also list `set_up_my_hand`.

Ownhand is meant for anything you send under your own name, even when you don't mention it. It isn't for code, notes or summaries, and you can opt out in any conversation. To make the habit stick in clients without memory, paste [`AGENTS.md`](AGENTS.md) into your `CLAUDE.md`, `AGENTS.md`, `.cursorrules` or custom instructions.

## Tools

| Tool | What it does |
|---|---|
| `write` | Rewrites a draft so it reads like you wrote it, fitted to the occasion. Keeps every fact. Returns the text and a `request_id`. |
| `send_feedback` | Reports what you did with a draft (`approved`, `edited` with the exact sent text, or `rejected`) so your Hand learns. |
| `get_hand` | Lists your Hands, or shows what one has learned: style card, rules, sample counts. |
| `create_hand` | Creates a Hand from texts you wrote yourself. |
| `update_hand` | Adds samples, sets how you describe your writing, pins, rejects or activates a learned rule, adds, edits or removes custom occasions, adds, edits or removes people, or rolls back to an earlier version. |
| `delete_hand` | Deletes a Hand and its history, permanently. |
| `get_account` | Shows your account, credit balance, recent charges and usage. |

Prompts: `set_up_my_hand`, `teach_my_voice`, `reply_to_thread`, `draft_email`.
Resources: `ownhand://guide` (the agent playbook), `ownhand://occasions` (the occasion table).

Agents should pass `recipient_name`, `recipient_email` when known, and `recipient_notes` to `write`, so drafts fit the person. `update_hand` can add, edit or remove people.

Agents should check `get_hand` for `custom_occasions` and prefer one when it fits. Add, edit or remove them with `update_hand`.

Occasions: `chat_dm`, `chat_channel`, `email_internal`, `email_formal`, `email_cold`, `email_warm`, `proposal_cold`, `proposal_warm`, `pr_description`, `review_comment`, `docs`, `status_update`.

## How learning works

1. Your agent calls `write` and shows you the draft. Nothing is sent without your OK.
2. You send it as is, edit it, or drop it.
3. Your agent calls `send_feedback` with exactly the text you sent.
4. Ownhand compares the draft with what you sent and updates your Hand. After you send feedback, a learning pass of a minute or two turns your edits and reasons into proposed rules, which become active once they're confirmed. A reason you state counts as strong evidence, but nothing is instant. The Hand's version number also goes up when you edit it, so don't read it as a sign that learning finished.

You can pin or reject any learned rule with `update_hand`. Learning from your feedback changes only your own Hand.

## Pricing

$1 free to start. Then you add credit and pay for the tokens you use, about half a cent per rewrite. No subscription and no monthly fee. Details: https://ownhand.dev/pricing.

## Privacy

- Stored: your account details, your Hands (samples, style card, learned rules), your requests and the feedback you send.
- Logged: which tools your agent calls, whether each call worked, how long it took and which app made it, never what the calls contain. Deleted after 90 days.
- Not stored: thread messages your agent passes as context. They are used for that one request.
- Not sold, not shared for advertising, and not used to train our own models.
- Writing and learning run on Muse Spark through OpenRouter's contributor tier, so the provider may use everything Ownhand processes (drafts, samples, thread context, feedback) to improve its models. Don't use Ownhand for text you want kept out of that.
- You can delete a Hand, revoke a key or disconnect an app at any time.

Read the full policy: https://ownhand.dev/privacy.

## Docs

- Docs: https://ownhand.dev/docs
- Agent guide: https://ownhand.dev/docs/agent-guide.md
- Connect: https://ownhand.dev/connect
- For agents: https://ownhand.dev/llms.txt

## What's in this repo

- `server.json`: the entry for the official MCP Registry
- `.claude-plugin/`, `.mcp.json`, `skills/ownhand/`: a Claude Code plugin and marketplace
- `gemini-extension.json`, `GEMINI.md`: a Gemini CLI extension
- `AGENTS.md`: the instruction block for any agent
- `examples/`: client configs

## License

The configs and docs in this repo are MIT licensed (see [LICENSE](LICENSE)). The Ownhand service itself is proprietary and run by Thalient Labs.

Contact: hi@thalientlabs.ai
