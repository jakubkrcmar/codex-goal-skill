# codex-goal

A Codex/Cursor skill for drafting durable, ready-to-paste Codex CLI `/goal` prompts.

The skill turns tasks, plans, repo context, and rough instructions into scoped goals with concrete stopping conditions, validation evidence, pause rules, and optional external PLAN.md patterns for long-running work.

## Why This Exists

Codex Goals are most useful when the objective is bigger than one turn but still has a real definition of done. In practice, rough goal prompts often fail in a few predictable ways: they ask the next agent to rediscover context, bundle unrelated work, treat time or token budget as completion, let plan files drift, or stop after a subagent/reviewer says "looks good" without checking the same evidence.

`codex-goal` is a small drafting layer for avoiding those failures. It turns an ambiguous request into a goal that says what to inspect first, what is in and out of scope, what evidence proves completion, when to use an external plan, and when to pause instead of guessing.

## Based On

This skill is based on three kinds of input:

- **Official Codex behavior and docs:** OpenAI's Goal guidance, CLI slash-command behavior, cookbook examples, and the current direct-objective length limit.
- **Real long-running Codex work:** practical runs where successful goals needed a visible stop condition, source-backed state checks, acceptance commands, and progress notes that survived context compaction.
- **Post-run failure analysis:** cases where goals were too vague, treated "budget exhausted" as done, relied on reviewer/subagent reports as proof, started autonomous external-model loops, or risked sending/updating external systems before explicit approval.

One concrete stress test was a draft-only CRM follow-up workflow: the useful goal was not "build an agent", but "produce a short reviewed approval queue from read-only data, with exact update plans, no live sends, no unapproved writes, and clear pause gates." That experience shaped the skill's external-plan pattern, reviewer-gate rules, and insistence that the main agent owns final validation.

## What It Does

- Drafts copy-pasteable `/goal ...` commands.
- Reviews and improves existing Codex goal prompts.
- Converts large or milestone-heavy tasks into external PLAN.md-backed goals.
- Keeps direct goal objectives under the 4,000-character Codex limit.
- Uses a reference guide for scope, validation, delegation, checkpoints, and pause conditions.

## When It Helps

Use it when a task should continue across turns, survive compaction, or be handed to another Codex run without losing the point. It is especially useful for:

- multi-step implementation, migration, debugging, or repair work
- research or audit tasks that need source coverage and uncertainty tracking
- workflows that touch external systems and need approval gates
- prompt/eval optimization that needs repeated runs and failure inspection
- goals that need an external plan with milestones, proof notes, and reviewer gates

For tiny one-turn tasks, a `/goal` usually adds ceremony. The skill should still help by keeping the prompt short or telling the agent to just do the work.

## Requirements

- Codex or Cursor skills support.
- Codex CLI with Goals enabled for actually running `/goal` commands.

## Install

Clone the repo into your skills directory:

```bash
git clone https://github.com/jakubkrcmar/codex-goal-skill.git ~/.codex/skills/codex-goal
```

Or copy the folder manually so the layout is:

```text
~/.codex/skills/codex-goal/
  SKILL.md
  references/codex-goal-guide.md
  agents/openai.yaml
```

## Usage

Ask your agent to draft a goal explicitly, for example:

```text
Use codex-goal to turn this task into a ready-to-paste /goal prompt.
```

For complex work, the skill may create or reference an external plan file and return a shorter `/goal` command that tells Codex how to follow that plan.

## Safety Model

- Draft-only by default: the skill should not set an active goal unless explicitly asked.
- Goals must include a verifiable stopping condition and concrete validation evidence.
- Destructive, external, ambiguous, or high-risk decisions should become pause conditions.
- External CLI models may be advisory only when explicitly approved; they should not become autonomous workers or authoritative plan editors.

## Changelog

### 2026-05-26

- Initial public release.
- Includes the core `codex-goal` skill, reference guide, and OpenAI agent metadata.
- Generalized the ops/business template for public use.
- Expanded README with the real-world failure modes and successful goal patterns behind the skill.

## License

MIT
