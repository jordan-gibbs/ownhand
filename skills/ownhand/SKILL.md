---
name: ownhand
description: Write as the user, in their own voice, with the Ownhand MCP server. Use for any text the user will send under their own name, such as a Slack message, DM, email, reply to a thread, post, comment, PR description or status update, even when they don't mention Ownhand, or when they ask to make AI-written text sound like them.
---

# Ownhand: write in the user's voice

Use the `ownhand` MCP tools for anything the user will send under their own name: emails, chat messages, replies, posts, comments, PR descriptions. They don't need to mention Ownhand. Do not rewrite it yourself.

Skip it for code, notes to self, summaries meant for the user to read, and text they want in someone else's voice. If they say not to use it, stop for the rest of the conversation.

## If the ownhand tools are not available

The plugin adds the server, but the user has to sign in once before its tools appear. If you can't see `write`, `get_hand` and the other ownhand tools, don't write the draft yourself as a stand-in. Tell the user how to connect, in one short message, then wait:

- Claude Code: run `/mcp`, pick `ownhand` (it shows as needing authentication), choose Authenticate, and sign in in the browser. Then ask again.
- Claude app (web, desktop, mobile): open Customize, then Connectors, find Ownhand and click Connect, sign in, pick a Hand and click Allow. In a chat, turn it on from the + menu under Connectors. Then ask again.
- Anything else: add the remote MCP server https://ownhand.dev/mcp and sign in. Setup for every client: https://ownhand.dev/connect

Write a plain draft without Ownhand only if the user says to go ahead without it.

1. Call `write` with the draft and the right `occasion`. Pick it from where the text will be posted and who reads it:
   `chat_dm`, `chat_channel`, `email_internal`, `email_formal`, `email_cold`, `email_warm`,
   `proposal_cold`, `proposal_warm`, `pr_description`, `review_comment`, `docs`, `status_update`.
   Call `get_hand` first and check `custom_occasions`: if one of the user's custom occasions fits (for example `linkedin_dm`), prefer it over a built-in.
   Leave it out if you truly cannot tell.
   When the message has a reader, pass `recipient_name`, `recipient_email` when known, and `recipient_notes` (what the user says about them), so drafts fit that person.
2. For a reply, set `is_reply: true` and pass the last messages of the thread verbatim, oldest first, as `context_messages`.
   If you cannot see the thread, ask the user to paste it. Never summarize or invent messages.
3. Show the draft exactly as returned. A short "(in your Hand)" once per conversation is enough; there's no need to explain Ownhand each time. Mention any `unresolved` items. Never send or post anything without the user's OK.
4. After the user acts on it, call `send_feedback` with the `request_id` and exactly the text that was sent:
   `approved` if sent unchanged, `edited` with `final_text` if they changed it, `rejected` with `was_sent: false` if not used.
   Pass the user's own words as `reason` when they say why. This is how the Hand learns. Always do it, without being asked, when the user says they sent it or shares what they sent. If they edit the draft in the chat and call it final, that edit is the sent text.
5. Put only what the user explicitly asked for (for example "shorter") in `user_instructions`. Never add your own style advice.

If a call says no Hand is selected or the Hand is missing, call `get_hand(list=true)`. If the user has no Hand, offer the routes you can do. If you have an email tool (such as Gmail),
offer to pull about 20 emails they sent. If you can reach Slack or Teams (a connector or a browser tool), offer about 20 of
their own messages. Ask first and accept a no. Keep only text the user wrote: cut quoted replies, forwards and signatures,
and skip AI drafts and anything sensitive. Or ask them to paste 5 to 10 things they wrote. A Hand works with fewer, but it sounds generic until it has about 5 to 20 real samples or has learned from feedback.
Then call `create_hand`, or `update_hand` with `add_samples` for an empty Hand, one batch per source with `samples_occasion`.
If a call says the user is out of credit, give them the billing link it returns.

On a client without saved instructions, offer once to remember this habit if the client has memory. Otherwise, say nothing about it.

Full guide: the `ownhand://guide` resource, or https://ownhand.dev/docs/agent-guide.md.
