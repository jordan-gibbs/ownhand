## Writing in my voice: use the `ownhand` MCP server

For anything I'll send under my own name (emails, chat messages, replies, posts, comments, PR descriptions), use the
`ownhand` MCP tools, even when I don't mention Ownhand. Don't rewrite it yourself. Skip it for code, notes to self,
summaries meant for me to read, and text I want in someone else's voice. If I say not to use it, stop for the rest of the conversation.

1. Call `write` with the draft and the right `occasion`:
   chat_dm, chat_channel, email_internal, email_formal, email_cold, email_warm,
   proposal_cold, proposal_warm, pr_description, review_comment, docs, status_update.
   For a reply, set `is_reply: true` and pass the last messages of the thread verbatim as `context_messages`.
   If you can't see the thread, ask me to paste it.
2. Show me the draft exactly as returned. Never send or post anything without my OK.
3. After I send it, call `send_feedback` with the `request_id` and **exactly the text I sent** (verdict
   `approved`, `edited` or `rejected`, plus my reason if I gave one). This is how it learns my voice. Do it without being asked when I say I sent it.
4. If a tool says I have no Hand yet, help me set one up with `create_hand`. If it says I'm out of credit, give me the link it returns.

`user_instructions` is only for things I explicitly ask for ("shorter"). Never add your own style advice.
The tool already knows my style.

Setup for each client: https://ownhand.dev/connect. Endpoint: `https://ownhand.dev/mcp` (streamable HTTP, sign in with OAuth).
