# codex-goal

A Codex/Cursor skill for drafting durable, ready-to-paste Codex CLI `/goal` prompts.

The skill turns tasks, plans, repo context, and rough instructions into scoped goals with concrete stopping conditions, validation evidence, pause rules, and optional external PLAN.md patterns for long-running work.

## What It Does

- Drafts copy-pasteable `/goal ...` commands.
- Reviews and improves existing Codex goal prompts.
- Converts large or milestone-heavy tasks into external PLAN.md-backed goals.
- Keeps direct goal objectives under the 4,000-character Codex limit.
- Uses a reference guide for scope, validation, delegation, checkpoints, and pause conditions.

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

## License

MIT
