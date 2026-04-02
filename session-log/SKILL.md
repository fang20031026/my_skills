---
name: session-log
description: Create or update concise per-session markdown work logs for ongoing projects. Use when the user wants to save, wrap up, archive, or summarize the current Codex conversation into `docs/session_logs/`, especially for cross-session handoff. This skill records only session-specific incremental work, decisions, blockers, and next actions; keep project-wide truth in `docs/project_log.md` via project-memory.
---

# Session Log

Maintain a concise, handoff-ready markdown log for one Codex session.

## Workflow

1. Identify the session scope.
   - Capture the user's intent for this conversation.
   - Capture the concrete goal of the session.
   - Determine whether this session changed code, produced artifacts, or only discussed plans.

2. Resolve the target file.
   - Default directory: `docs/session_logs/`
   - Default filename: `YYYY-MM-DD_HHMM.md`
   - If the user explicitly wants a daily rollup, use `YYYY-MM-DD.md` and append a new top-level session entry instead of overwriting prior notes.

3. Write only session-local deltas.
   - Record what happened in this session.
   - Record outcomes, not just activities.
   - Distinguish implemented changes from discussion-only conclusions.
   - Record blockers and the exact reason the session stopped.

4. Link back to project memory.
   - Treat `docs/project_log.md` as the project-wide source of truth.
   - State whether the important conclusions of this session were already synced to `docs/project_log.md`.
   - If not synced, say so explicitly.

5. End with a direct handoff.
   - Write concrete next actions.
   - Include a prompt that a future Codex instance can use to resume work quickly.

## File Contract

- Session logs live under `docs/session_logs/`
- One file per session by default
- Session logs are additive records, not replacements for `docs/project_log.md`
- Do not rewrite older session logs unless the user asks

## Canonical Structure

Use this exact section order and headings:

1. `# Session Log - <Project Name> - <YYYY-MM-DD HH:MM>`
2. `## Context`
3. `## User Intent`
4. `## Session Goal`
5. `## Completed`
6. `## Implemented`
7. `## Discussed Only`
8. `## Decisions`
9. `## Current Blockers`
10. `## Stop Reason`
11. `## Next Actions`
12. `## Handoff Prompt`

## Section Rules

### Context

Include:

- `Date`
- `Branch`
- `Topic`
- `Related Files`
- `Synced To Project Memory: Yes/No`

If a value is unknown, write `Unknown` instead of guessing.

### User Intent

State the user's actual problem or request in one to three bullets.

### Session Goal

State the concrete goal for this session, not the whole project.

### Completed

Each bullet must describe:

- the action
- the result

Good:

- Checked the `project-memory` skill and confirmed it requires `docs/project_log.md` as the primary memory entrypoint.

Bad:

- Read some skill files.

### Implemented

List only things that were actually created, edited, executed, or saved during this session.

Examples:

- Created a local draft at `tmp_project_memory_review/project_log.md`.
- Updated `~/.codex/skills/session-log/SKILL.md`.

If nothing was implemented, write:

- No files or persistent artifacts were created in this session.

### Discussed Only

List designs, ideas, or decisions that were discussed but not yet implemented.

### Decisions

For each non-trivial decision, include:

- decision
- why
- impact

Use short bullets, not long paragraphs.

### Current Blockers

List active blockers that matter for the next session.

If there are no blockers, write:

- None.

### Stop Reason

State why work stopped at this point.

Examples:

- Waiting for the user to review the proposed skill design.
- Stopped after creating the first draft so the user can inspect it before upload.

### Next Actions

Write concrete, executable items.

Good:

- [ ] Create `docs/session_logs/_template.md` in the target project.
- [ ] Update `docs/project_log.md` to mention the new session-log workflow.

Bad:

- [ ] Keep improving things.

### Handoff Prompt

Provide a short prompt a future Codex instance can use directly.

The prompt should usually instruct Codex to read:

- `docs/project_log.md`
- the latest relevant file under `docs/session_logs/`

## Content Rules

- Prefer bullets over long paragraphs.
- Record facts from this session only.
- Do not duplicate the entire project state from `docs/project_log.md`.
- Do not claim implementation happened unless files or commands confirm it.
- Make uncertainty explicit.
- Optimize for fast handoff, not narrative completeness.

## Coordination With Project Memory

Use this skill together with `project-memory` when the user wants both:

- a durable project-wide state file
- a session-by-session audit trail

Recommended split:

- `project-memory` updates `docs/project_log.md`
- `session-log` writes `docs/session_logs/*.md`

Recommended end-of-session order:

1. Write the session log
2. Promote durable conclusions into `docs/project_log.md` if requested
3. Return a handoff prompt

## Prompt Recipes

### 1) Save This Session

Use when the user says "write a session log", "save this conversation", or "wrap up this session".

```text
Summarize this Codex session into docs/session_logs using the session-log skill.
Record user intent, completed work, implemented artifacts, discussed-only items, decisions, blockers, stop reason, next actions, and a handoff prompt.
Do not duplicate full project state from docs/project_log.md.
```

### 2) Save Session And Update Project Memory

Use when the user wants both logs updated.

```text
Write the session log for this conversation under docs/session_logs, then update docs/project_log.md with only the durable project-state changes.
Keep docs/project_log.md as the source of truth for project-wide status.
```

### 3) Daily Rollup Mode

Use only when the user explicitly wants one file per day.

```text
Append this session to today's docs/session_logs/YYYY-MM-DD.md file using the session-log structure.
Preserve prior entries and add a new top-level session block for this conversation.
```

## Quality Bar

Before finishing, verify:

- The file clearly says what the user wanted this session.
- Completed items include outcomes, not just actions.
- Implemented and Discussed Only are separated.
- The stop reason explains why the session ended here.
- Next Actions are specific enough for immediate execution.
- The handoff prompt is directly reusable.

## Non-Goals

Do not turn the session log into:

- a full transcript
- a duplicate of `docs/project_log.md`
- generic brainstorming notes without outcomes
- a changelog for unrelated historical work
