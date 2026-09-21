---
name: intamos-support-triage
description: "Sort the latest tickets by priority, draft replies in the business's voice, escalate with a reason a human can act on. Use for support queues, tickets, customer complaints or 'what needs an answer today'."
---

# Support triage

Read first, always:

1. `intamos://whoami` — the scopes this connection holds; with `read` only, sort and draft, do not send.
2. `intamos://workspace/profile` — the business's voice, products and policies. Replies are written in THEIR voice; check `searchKnowledge` before stating a price, a policy or a timeline.

## Sort

- `listTickets` — the most recent tickets (up to 15; the connector does not filter by status or expose timestamps). Rank by priority, then status (open before pending before closed — use the business's own status words from the profile), then by `ref` order.
- If the user names the customer, `searchContacts` by name, email or phone and `getContact` for company, owner and recent notes.

Show the queue as a table: ticket (`ref`), subject, status, priority, proposed action (reply / escalate / close).

## Draft

For each ticket you propose to reply to, draft the reply in the business's voice: short, specific, one next step, no promise the profile or knowledge base does not back. Show the draft; do not send it yet.

## Act (only with `write`, and only after a yes per ticket)

- `replyToTicket` with the approved draft, unchanged.
- `updateTicket` to set priority or status when the user agrees.
- `addTicketNote` to escalate: the note names what is needed, from whom, and by when — a human reads it cold.

One write per confirmation. A scope refusal ends the acting section: point to https://intamos.com/workspace/settings/claude.
