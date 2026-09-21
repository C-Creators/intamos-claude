# intamos for Claude Code

Your intamos CRM inside Claude Code: **Maggie** as a subagent, three workflow
skills, and the intamos connector — contacts, deals, pipeline, tickets,
appointments, automations, prospects and knowledge base.

> **This repository is a build artifact.** The source of truth is
> `packages/claude-plugin` in the intamos monorepo, which is private; every push
> there overwrites this repo, so a pull request here would be erased. Bugs,
> questions and requests go to <support@intamos.com>, or to
> <https://intamos.com> — the team answers from there.

## Install

```
/plugin marketplace add C-Creators/intamos-claude
/plugin install intamos@intamos
```

Claude Code asks for your **intamos connector token** when it enables the
plugin. Mint one at <https://intamos.com/workspace/settings/claude>: choose a
label and the scopes, copy the token — it starts with `intamos_mcp_` and is
shown once.

Updates: a change to this plugin ships as a version bump in `plugin.json` (and
the matching entry in `marketplace.json`); `/plugin update intamos@intamos`
picks it up.

## What the token grants

A token is bound to **you, in one team**, and capped by the scopes you chose:

| Scope | What Claude can do |
| --- | --- |
| `read` | Read contacts, deals, tickets, appointments and metrics |
| `write` | Create and update records, move deals, schedule appointments, start prospect searches (paid) |
| `admin` | Delete records, invite people, switch modules on and off |

Your role in the team is enforced on every call — a token never grants more
than you can do in the app. Every write asks you first in Claude Code, and lands
in the workspace's activity timeline as "via Claude".

## Revoke

Open <https://intamos.com/workspace/settings/claude> and revoke the connection.
The next call from Claude Code is refused. Mint a new token any time.

## Without the plugin

If you would rather not have the plugin own the connection, do not install the
plugin's server twice — connect the server directly instead:

```
claude mcp add --transport http intamos https://intamos.com/api/mcp --header "Authorization: Bearer $INTAMOS_MCP_TOKEN"
```

The Connect page prints that line with your token filled in.

## What is in the plugin

- `agents/maggie.md` — Maggie, the intamos CRM assistant. Claude delegates to
  her for anything about your business's records. She reads the CRM and acts
  through the connector; she does not touch your files.
- `skills/intamos-pipeline-review` — walk the pipeline: stale deals, missing
  next steps, what to move, what to close.
- `skills/intamos-lead-triage` — qualify and route new leads against your own
  stages and terminology.
- `skills/intamos-support-triage` — sort open tickets by urgency, draft replies
  in your voice, escalate with a reason a human can act on.
