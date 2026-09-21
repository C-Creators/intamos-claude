---
name: intamos-lead-triage
description: "Qualify and route new leads against this business's own stages and terminology. Use when leads, prospects or inbound requests need sorting, scoring or turning into deals."
---

# Lead triage

Read first, always:

1. `intamos://whoami` — the scopes this connection holds; with `read` only, qualify and recommend, do not create.
2. `intamos://workspace/profile` — the business's stages, terminology and what a qualified lead means to THEM. Score against their definition, not a generic one.

## For each lead the user gives you (name, company, message, or a pasted email)

1. `searchContacts` by name, email or phone (it does not search by company) — never create a duplicate. If found, `getContact` for company, owner and recent notes (it does not carry the contact's deals; `getPipeline` names the top deals by value if you need them).
2. Qualify in three lines: fit (does the business serve this kind of customer, per the profile), intent (what they asked for, in their words), urgency (a date or a trigger, if any).
3. Recommend ONE of: create a deal in stage X (the first stage from the profile unless the message clearly warrants a later one), add a task to reply, or pass — with the reason.

Present all leads as a short table before doing anything: lead, fit, intent, recommendation.

## Act (only with `write`, and only after a yes per lead)

- `createContact` when the person is new — name, email, phone, company as given; nothing invented.
- `createDeal` on the contact, in the stage you recommended, titled the way the profile's examples are titled.
- `createTask` for the reply, due today or tomorrow, assigned to the user unless they name someone.

One write, then the next question. A scope refusal ends the acting section: point to https://intamos.com/workspace/settings/claude.
