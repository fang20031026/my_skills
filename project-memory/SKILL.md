---
name: project-memory
description: Structured project memory and session handoff for Codex CLI. Use when users want to continue a project across new chats/devices, restore context from markdown logs, track Done/In Progress/Pending status, preserve architecture decisions with rationale, enforce non-negotiable constraints, and generate next executable steps. Trigger on requests like “restore context”, “continue from project_log.md”, “save progress”, “summarize session”, “update project memory”, or “next step”.
---

# Project Memory Skill

Maintain an AI-readable project state machine in markdown so development can continue across days, devices, and fresh chats without losing context.

## Workflow (Always Follow)

1. **Restore Context First**
   - Read `docs/project_log.md` before proposing code changes.
   - Extract:
     - current state
     - active work item
     - hard constraints
     - recent decisions and rationale
     - next executable steps
   - If the file is missing, create it from the template in this skill.

2. **Plan Against Constraints**
   - Treat `Constraints (Non-Negotiable)` as hard rules.
   - Do not violate architecture guardrails.
   - If a requested action conflicts with constraints, stop and ask for confirmation.

3. **Execute Focused Work**
   - Continue from `In Progress` and `Next Steps`.
   - Avoid unrelated refactors.
   - Preserve prior design intent unless explicitly asked to redesign.

4. **Save Session Memory**
   - Update `docs/project_log.md` at session end:
     - Move completed work to `Done`
     - Refresh `In Progress` and `Pending`
     - Append `Decisions (with rationale)`
     - Rewrite `Next Steps` as concrete, executable checklist
     - Add `Session Summary`

---

## Required File Contract

Primary memory file:

- `docs/project_log.md` (required)

Optional split files (only if project grows):

- `docs/decisions.md`
- `docs/constraints.md`

If split files exist, keep `docs/project_log.md` as the single entrypoint with links and a concise snapshot.

---

## `docs/project_log.md` Structure (Canonical)

Use this exact section order and headings:

1. `# Project Memory - <Project Name>`
2. `## Snapshot`
3. `## Constraints (Non-Negotiable)`
4. `## Architecture Guardrails`
5. `## Progress`
   - `### Done`
   - `### In Progress`
   - `### Pending`
6. `## Decisions (with Rationale)`
7. `## Next Steps (Executable)`
8. `## Session Summary`
   - `### This Session`
   - `### Handoff Prompt`

Keep entries concise, dated, and implementation-oriented.

---

## Content Rules

- Prefer bullet points over long paragraphs.
- Include dates in `YYYY-MM-DD`.
- Use stable IDs for decisions: `D-YYYYMMDD-01`, `D-YYYYMMDD-02`, etc.
- For each decision, include:
  - chosen option
  - rejected option(s)
  - reason
  - impact
- `Next Steps` must be concrete and code-oriented (file/function/task level).
- Never delete historical decisions unless explicitly asked.
- Never rewrite history silently; append updates.

---

## Prompt Recipes (Use Verbatim or Slightly Adapt)

### 1) Restore Context

Use when user says “continue”, “restore context”, or references `project_log.md`.

```text
Read docs/project_log.md.
Summarize current state, constraints, active in-progress task, and best next executable step in 5 bullets.
Then continue implementation without violating Constraints or Architecture Guardrails.
```

### 2) Save Progress

Use when user says “summarize”, “save progress”, “wrap up”, or end-of-session.

```text
Summarize this session and update docs/project_log.md:
- Move finished items to Done
- Refresh In Progress/Pending
- Append Decisions with rationale and tradeoffs
- Rewrite Next Steps as an executable checklist
- Add Session Summary
Do not remove historical records.
```

### 3) Next Best Action

Use when user is blocked or asks “what next”.

```text
Based on docs/project_log.md only, provide the single best next step with exact files/functions to edit, expected output, and risk checks.
```

---

## Conflict Policy

When request conflicts with memory:

1. Quote the conflicting constraint/decision briefly.
2. Ask one clear confirmation question.
3. Offer two options:
   - **A:** follow existing memory
   - **B:** override and record a new decision

If override is chosen, append a new decision entry explaining why the prior decision changed.

---

## Initialization Template (Create if Missing)

If `docs/project_log.md` does not exist, create this starter content:

```markdown
# Project Memory - <Project Name>

## Snapshot
- Last Updated: <YYYY-MM-DD>
- Current Phase: <phase>
- Current Branch: <branch>
- Owner: <name>

## Constraints (Non-Negotiable)
- [C1] <hard constraint>
- [C2] <hard constraint>
- [C3] <hard constraint>

## Architecture Guardrails
- <guardrail 1>
- <guardrail 2>

## Progress
### Done
- [<YYYY-MM-DD>] Initialized project memory.

### In Progress
- [P1] <active task>
  - Status: 0%
  - Files: <paths>
  - Blockers: <none>

### Pending
- [ ] <next task>
- [ ] <later task>

## Decisions (with Rationale)
- [D-<YYYYMMDD>-01] <decision summary>
  - Why: <reason>
  - Tradeoff: <tradeoff>
  - Impact: <impact>

## Next Steps (Executable)
1. <step 1>
2. <step 2>
3. <step 3>

## Session Summary
### This Session
- Created initial memory file.

### Handoff Prompt
Read docs/project_log.md, follow Constraints and Architecture Guardrails, continue from In Progress and execute Next Steps only.
```

---

## Quality Bar for Updates

Before finishing any session memory update, verify:

- Constraints are present and unchanged unless intentionally updated.
- In Progress contains exactly what is currently active.
- Next Steps are actionable within one coding session.
- At least one rationale-backed decision exists if non-trivial choices were made.
- Handoff Prompt is present.

---

## Non-Goals

Do not turn project memory into:

- personal diary
- verbose meeting notes
- generic brainstorming dump

Keep it engineering-focused and executable by another Codex instance.
