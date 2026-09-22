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

Add the connector directly:

```
claude mcp add --transport http intamos https://intamos.com/api/mcp
```

or install the plugin:

```
/plugin marketplace add C-Creators/intamos-claude
/plugin install intamos@intamos
```

Either way, there is nothing to copy. Run `/mcp`, select **intamos**, and sign
in in the browser — or run `claude mcp login intamos` (`--no-browser` over
SSH prints a URL to open elsewhere and asks you to paste the redirect back).
Headless `claude -p` runs cannot sign in; log in once from an interactive
session first. `claude mcp logout intamos` clears the connection on this
machine (see **Revoke** below for ending it everywhere).

Updates: a change to this plugin ships as a version bump in `plugin.json` (and
the matching entry in `marketplace.json`); `/plugin update intamos@intamos`
picks it up.

## What a connection grants

A connection is bound to **you, in one team**, and capped by the scopes it was
authorized with:

| Scope | What Claude can do |
| --- | --- |
| `read` | Read contacts, deals, tickets, appointments and metrics |
| `write` | Create and update records, move deals, schedule appointments, start prospect searches (paid) |
| `admin` | Delete records, invite people, switch modules on and off |

Signing in requests `read write`; `admin` is only available on a connector
token (see below). Your role in the team is enforced on every call — a
connection never grants more than you can do in the app. Every write asks you
first in Claude Code, and lands in the workspace's activity timeline as "via
Claude".

## Revoke

`claude mcp logout intamos` forgets the sign-in on **this machine** — it clears
the stored credentials locally and revokes nothing server-side, so a token
already issued keeps working until it expires.

To end the connection **everywhere**, revoke it from the Connect page at
<https://intamos.com/workspace/settings/claude>: that kills the grant and every
token issued under it immediately, which is what you want if a laptop is lost or
a machine is no longer yours. Sign in again, or mint a new token, any time.

## Scripts and headless runs

A headless `claude -p` run or a script cannot sign in interactively. Mint a
connector token at <https://intamos.com/workspace/settings/claude> instead,
and connect with it directly:

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
