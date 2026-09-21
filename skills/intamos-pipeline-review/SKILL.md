---
name: intamos-pipeline-review
description: "Walk the pipeline: stale deals by stage, missing next steps, what to move, what to close. Use when asked to review, clean up or report on the pipeline, or when someone asks what is stuck."
---

# Pipeline review

Read first, always:

1. `intamos://whoami` — the scopes this connection holds. With `read` only, this is a report; say so up front and skip the "act" section.
2. `intamos://workspace/profile` — the business's stage names and terminology. Use them verbatim; never invent a stage.

## Gather

- `getPipeline` — per-stage counts and total value, plus the top deals by value (name, value, stage — this call carries no owner and no last-activity field).
- `listTasks` — open tasks, so a top deal can be checked by title for a follow-up.
- `getRecentActivity` — the most recent activity events (up to 20, newest first — not a fixed date range), so a top deal can be checked by name for recent movement.

One call each. Do not page through the same data twice.

## Report

Say up front that this covers the stage totals and the deals `getPipeline` names as its "top deals" by value — not literally every open deal — and that none of these calls carries a deal's owner.

Group by stage, in the profile's order. For each stage, from the aggregates: count and total value. For each top deal in that stage, name it and call it **stale** when no `listTasks` title names it AND no `getRecentActivity` event mentions it. Keep it to what changes a decision; a healthy stage is one line.

End with three lists, each item one line with the deal name:

- **Move** — deals whose activity shows they belong in a later stage.
- **Follow up** — stale deals with no matching task; propose one.
- **Close** — stale deals with no matching task or activity, that look abandoned.

## Act (only with `write`, and only after a yes)

Ask before EACH change, one at a time, in the order the user picks:

- `moveDeal` to move a deal to a stage from the profile.
- `createTask` to add the follow-up, due within the week, on the deal's contact.
- To close a deal: the connector has no tool that sets a deal's status — `updateDeal` only changes name, value, priority and expected close date, never status. If the profile lists a closed/lost stage, `moveDeal` the deal there instead. If it does not, `createTask` asking the user (or whoever they name) to close it from inside the workspace, due today — say plainly that the connector cannot close it directly.

Never batch. If a call is refused for scope, stop acting and say where to widen the connection: https://intamos.com/workspace/settings/claude.
