# Installing the Ownhand MCP server

Ownhand is a remote MCP server. There is nothing to download, build or run locally.

- Server name: `ownhand`
- URL: `https://ownhand.dev/mcp`
- Transport: streamable HTTP (in Cline, `"type": "streamableHttp"`)
- Auth: OAuth 2.1 with dynamic client registration. No API key is needed.

## Steps

1. Add this entry to the `mcpServers` object in `cline_mcp_settings.json`. Keep any servers already there.

   ```json
   {
     "mcpServers": {
       "ownhand": {
         "type": "streamableHttp",
         "url": "https://ownhand.dev/mcp"
       }
     }
   }
   ```

2. Save the file. Cline connects and the server asks for sign-in. Tell the user to click to authenticate, sign in or sign up in the browser, pick a Hand (their voice profile), and click Allow. New accounts start with $1 of free credit.

3. Check the connection by calling `get_hand` with no arguments. If it returns no Hands, ask the user for 5 to 10 things they wrote themselves (messages, emails, PR descriptions they actually sent) and call `create_hand` with them.

## Notes

- OAuth: do not add an `Authorization` header for the normal setup. The first request returns 401 with a `WWW-Authenticate` header that points to `https://ownhand.dev/.well-known/oauth-protected-resource/mcp`, and the client runs the browser sign-in from there.
- If the client cannot do OAuth, the user can create an API key at https://ownhand.dev/dashboard#keys and you can add `"headers": { "Authorization": "Bearer <OWNHAND_API_KEY>" }` to the entry. Never write a key into a file that is committed to a repository.
- Using it: call `write` with the draft and an `occasion`, show the user the result, and do not send anything without their OK. After they send it, call `send_feedback` with the `request_id` and the exact text that was sent.
- Full agent guide: https://ownhand.dev/docs/agent-guide.md. Setup for other clients: https://ownhand.dev/connect
