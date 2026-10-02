## Writing in my voice: use the `ownhand` MCP server

When I ask you to write something "in my voice", draft a message, email, reply or PR description for me to send,
or make AI-sounding text sound like me, use the `ownhand` MCP tools. Don't rewrite it yourself.

1. Call `write` with the draft and the right `occasion`:
   chat_dm, chat_channel, email_internal, email_formal, email_cold, email_warm,
   proposal_cold, proposal_warm, pr_description, review_comment, docs, status_update.
   For a reply, set `is_reply: true` and pass the last messages of the thread verbatim as `context_messages`.
   If you can't see the thread, ask me to paste it.
2. Show me the draft exactly as returned. Never send or post anything without my OK.
3. After I send it, call `send_feedback` with the `request_id` and **exactly the text I sent** (verdict
   `approved`, `edited` or `rejected`, plus my reason if I gave one). This is how it learns my voice. Always do it.
4. If a tool says I have no Hand yet, help me set one up with `create_hand`. If it says I'm out of credit, give me the link it returns.

`user_instructions` is only for things I explicitly ask for ("shorter"). Never add your own style advice.
The tool already knows my style.

Setup for each client: https://ownhand.dev/connect. Endpoint: `https://ownhand.dev/mcp` (streamable HTTP, sign in with OAuth).
