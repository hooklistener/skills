---
name: setup
description: Set Hooklistener up for this project. Signs in, finds where the app receives webhooks or sends emails, proves one works end to end, and notes how to test it again in CLAUDE.md.
disable-model-invocation: true
---

# Set up Hooklistener for this project

Work through these steps in order and tell the user what you're doing at each one. Follow the `hooklistener` skill for how to use the tools.

1. **Connect.** Call `list_endpoints`. If the Hooklistener tools aren't available or the call fails with an authentication error, ask the user to run `/mcp`, select `hooklistener` and sign in with the browser (a free account works), then stop until they say it's done.

2. **Look for what's already set up.** Read the `## Hooklistener` section of CLAUDE.md or AGENTS.md, if there is one, and the endpoints `list_endpoints` returned. If an anonymous endpoint from earlier in this conversation is still around, keep it with `claim_endpoint` rather than creating a new one.

3. **Find what to test.** Search the code for:
   - webhook receivers: routes like `/webhooks/*`, provider SDK calls such as `stripe.webhooks.constructEvent`, `verifyWebhook`, `X-Hub-Signature`;
   - emails the app sends: signup confirmation, login codes, magic links, password resets;
   - real-time clients: WebSocket, Socket.IO, MQTT or SSE connections.

   Summarize what you found and ask the user which one to prove first. If there's nothing, say so and ask what they want to test.

4. **Prove it.** For a webhook, create an endpoint named after the sender and project (`Stripe · <project>`), have the user send a test event, `wait_for_request`, then send the captured request to the local handler with curl and report what it answered. For an email, create an inbox and run the email recipe from the `hooklistener` skill. If the local app isn't running, ask the user to start it rather than starting long-running servers yourself.

5. **Leave a note.** Add or update the `## Hooklistener` section in CLAUDE.md (or AGENTS.md if that's what the project uses) with the endpoint or inbox, the handler file and the exact steps to test again. No tokens or signing secrets.

End with a short summary: what's set up, its URL, and the one-line way to test it again.
