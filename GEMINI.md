# Ownhand

Ownhand is connected as the `ownhand` MCP server. It writes text in the user's own voice (their Hand), fitted to the occasion, and learns from what they actually send.

If the tools ask for sign-in, tell the user to run `/mcp auth ownhand`.

## Playbook

Use the `ownhand` MCP tools for anything the user will send as themselves. Do not rewrite it yourself.

1. Call `write` with the draft and the right `occasion`. Pick it from where the text will be posted and who reads it:
   `chat_dm`, `chat_channel`, `email_internal`, `email_formal`, `email_cold`, `email_warm`,
   `proposal_cold`, `proposal_warm`, `pr_description`, `review_comment`, `docs`, `status_update`.
   Leave it out if you truly cannot tell.
2. For a reply, set `is_reply: true` and pass the last messages of the thread verbatim, oldest first, as `context_messages`.
   If you cannot see the thread, ask the user to paste it. Never summarize or invent messages.
3. Show the draft exactly as returned. Mention any `unresolved` items. Never send or post anything without the user's OK.
4. After the user acts on it, call `send_feedback` with the `request_id` and exactly the text that was sent:
   `approved` if sent unchanged, `edited` with `final_text` if they changed it, `rejected` with `was_sent: false` if not used.
   Pass the user's own words as `reason` when they say why. This is how the Hand learns. Always do it.
5. Put only what the user explicitly asked for (for example "shorter") in `user_instructions`. Never add your own style advice.

If a call says no Hand is selected or the Hand is missing, call `get_hand(list=true)`. If the user has no Hand, ask for 5 to 10 things they wrote themselves
(Slack messages, emails, PR descriptions; no AI drafts) and call `create_hand`, or `update_hand` with `add_samples` for an empty Hand.
If a call says the user is out of credit, give them the billing link it returns.

Full guide: the `ownhand://guide` resource, or https://ownhand.dev/docs/agent-guide.md.
