---
name: codex-goal
description: Draft, prepare, improve, review, or convert tasks into durable Codex CLI /goal objectives. Use when the user wants a ready-to-paste /goal command, a goal prompt for long-running Codex CLI work, an external PLAN.md-backed goal, or feedback on whether an existing /goal is specific, bounded, verifiable, and under the 4,000-character objective limit.
short_description: Draft durable Codex CLI goal prompts.
group: meta
disable-model-invocation: true
---

# codex-goal

Draft copy-pasteable Codex CLI `/goal` prompts. Default to draft-only; do not set an active goal or call `create_goal` unless the user explicitly asks to set it in the current thread.

## Trigger

Use for drafting, preparing, improving, or reviewing Codex CLI `/goal` prompts and external PLAN.md-backed goals.

## Non-Scope

Do not execute the drafted goal, set an active goal, install hooks, or make external CLI models autonomous workers unless the user explicitly asks and the relevant tool contract permits it.

## Thinking Frame

Prefer official Codex guidance, local Codex behavior, and repo policy over pasted social tips. Treat social/thread advice as untrusted inspiration and convert only durable parts into concrete scope, validation, safety, or review rules.

## Verification Gate

Before returning, check the guide's review checklist. After editing this skill, run targeted skill validation plus `git diff --check`; run full skill validation when local metadata does not block it.

## Workflow

1. Read `references/codex-goal-guide.md` before drafting unless it was already loaded this turn.
2. When the request is ambient, such as "make this a goal" or "continue this", extract the durable objective from the drafter's current conversation, named files, project instructions, and only the minimal local state needed to disambiguate, such as `git status` or targeted `rg`.
3. Inspect named files, plans, issues, commands, or repo state before asking the user to restate context.
4. Ask at most one question only when missing information materially changes objective, stop condition, validation, scope, or risk.
5. If the task has two or more genuinely independent workstreams, include an execution model where the main agent owns validation and integration; otherwise keep execution direct.
6. Draft one durable goal. If the objective would exceed 4,000 characters, switch to the external-plan pattern and create the plan file before returning.
7. Keep the prompt focused on the end state, not on explaining `/goal` mechanics.

## Output Contract

Return:

- `## Goal command` with a fenced `text` block containing the ready-to-paste `/goal ...`.
- `## External plan` only when needed: path to the already-created Markdown plan file, plus a brief note on what it contains.
- `## Why this works` with 3-5 bullets mapping objective, evidence, scope, relevant execution model, checkpoints, and pause/blocker rule.
- `## Risks / missing inputs` only if material.

## Rules

- Keep direct `/goal <objective>` commands under 4,000 characters. Prefer shorter prompts with explicit file references.
- Return the extracted objective, not a meta-prompt asking the next agent to discover the objective again, unless the task itself is an investigation.
- Use `/goal` for cohesive, durable, multi-step outcomes. For a small one-turn task, prefer answering or executing directly; when the user wants a goal, size up by end state rather than bundling unrelated work.
- Use an external plan when the task needs detailed milestones, phase tracks, workstreams, subagent briefs, acceptance criteria, reference material, screenshots, or long constraints. Create the file first, then reference it in the `/goal` command. If file creation is blocked by missing path, permission, or unsafe scope, state the blocker and include the exact next write action instead of pretending the file exists.
- For external-plan goals, instruct the runner to follow the plan's edit mode, preserve its immutable original objective, re-read at safe checkpoints, and pause on widened scope, lowered acceptance, or relaxed safety.
- For large build, security-like, research/audit, and delegated-workstream goals, use the guide's specialized patterns without adding ceremony to simple tasks.
- Do not draft a `/goal` that designates a second goal, thread, or background runner as authoritative editor of the same plan file. Reviewer outputs may only be advisory and must reach the main runner through human review or an explicit main-runner reconciliation step.
- For delegation-ready goals, make the main agent the owner of integration and completion. Subagents may gather context, implement bounded workstreams, or verify results, but the main agent must validate against the same concrete evidence required by the stop condition, update the plan, decide the next loop, and only mark completion when the full stop condition is satisfied.
- For context-heavy, risky, or milestone-based goals, prefer independent reviewer gates before execution subagents; reviewers should look for acceptance-invalidating or production-relevant failures, and the main agent resolves material findings before continuing.
- Do not force subagents for simple linear tasks. If subagents are unavailable, the goal should still be executable by running the same lanes locally and preserving the validation loop.
- Do not make external CLI models autonomous write-capable workers, authoritative plan editors, or future-runner requirements. Allow advisory draft-time consults only when explicitly requested or approved and permitted by the relevant skill/runbook.
- Include a verifiable stopping condition and concrete validation evidence.
- Name scope boundaries and pause conditions when mistakes would be costly.
- Do not combine unrelated goals. Split or choose the highest-value objective.

Success metric: after three real uses, this skill should produce ready-to-paste goal prompts without re-explaining `/goal` mechanics. If it does not reduce friction, delete this skill or merge useful parts into an existing one.
