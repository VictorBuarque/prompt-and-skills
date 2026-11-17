# Copilot Prompts Repository

Centralized repository for authoring, versioning, and evolving AI agent prompts and Cursor skills with engineering-grade standards.

## Overview

This repository is the source of truth for prompt assets and agent skills.  
It focuses on implementation-oriented prompts and review workflows that drive real code changes, not generic brainstorming output.

## Goals

- Establish a single source of truth for prompt and skill assets.
- Improve prompt quality over time through versioned iterations.
- Standardize prompt structure and response expectations.
- Enable cross-project and cross-team reuse.
- Keep the skill Cursor loads identical to the copy in this repo.

## Repository Structure

```text
.
├── prompts/
│   ├── prompt-agent.md          # Agent: implement the change
│   ├── prompt-ask.md            # Ask: read-only answers
│   ├── prompt-plan.md           # Plan: implementation plan, no code
│   ├── prompt-study.md          # Study: teach a topic
│   └── pull_request_template.md # PR body from the pr-markdown skill
├── skills/
│   ├── feature-review/
│   │   └── SKILL.md             # Review the feature, then verdict
│   └── pr-markdown/
│       └── SKILL.md             # Write the author's proof to PR.md
└── README.md
```

## Skills

A skill is a directory with a `SKILL.md`: YAML frontmatter plus the procedure. Cursor does not load the `skills/` folder in this repo. It loads `~/.cursor/skills/<name>/` (every project) or `.cursor/skills/<name>/` inside a project.

This repo is the versioned copy. After creating or editing a skill here, refresh the installed folder:

```bash
cp -R skills/feature-review ~/.cursor/skills/feature-review
cp -R skills/pr-markdown ~/.cursor/skills/pr-markdown
```

On Windows that directory is `%USERPROFILE%\.cursor\skills`. If the installed folder is left stale, Cursor keeps running the old text.

| Skill | Role |
| --- | --- |
| `feature-review` | Reviews the whole feature, not only the diff. Classifies each path, requires the four safety anchors and the PR proofs, and separates merge-ready from launch-ready. |
| `pr-markdown` | Writes the pull request title and body to `PR.md` at the repo root before opening a GitHub PR. That file is the author's proof. It does not contain the review verdict. |

`pr-markdown` fills the four anchors, Proof, Confidence, and Experiment, using the names from `feature-review`. `feature-review` re-checks those sections and adds the verdict.

The anchor headings stay exactly:

1. O que mudou? (What changed)
2. O que não deve ser mudado? (What must not change)
3. Até onde o fracasso pode ir? (How far can failure travel?)
4. Podemos bloquear ou reverter isso? (Can we gate or roll it back?)

### Skill authoring

- Path: `skills/<kebab-name>/SKILL.md`
- Frontmatter: `name` (lowercase, hyphens, max 64 characters) and a third-person `description` that says what the skill does and when to use it
- Keep `SKILL.md` under 500 lines
- Use one term per concept. The review pair shares the anchor headings and the proof names (Tests, Runtime path, Visual, Left unmodified)

## Default stack (high level)

Prompts assume **React + TypeScript + Node.js** unless otherwise stated.  
**Web:** Next.js (App Router) by default.  
**Mobile (iOS/Android):** **React Native** with **Expo** as the default framework when the solution is mobile.  
Notifications (push/local): **expo-notifications** when applicable.

## Prompt Design Principles

Each prompt should be:

- **Deterministic**: clear defaults and minimal ambiguity.
- **Actionable**: optimized for implementation workflows.
- **Context-aware**: respects existing codebase conventions first.
- **Verifiable**: includes explicit validation and quality criteria.
- **Maintainable**: easy to evolve without breaking behavior.

## Authoring Standard

When creating a new prompt in `prompts/`, include at least:

1. **Purpose**
2. **Target stack and assumptions**
3. **Decision rules** (when to ask vs. when to assume)
4. **Execution lifecycle** (discover, plan, implement, verify, finalize)
5. **Output format contract**
6. **Definition of Done (DoD)**

## Naming Convention

- Prompt files: `kebab-case.md`
- Skill folders: `kebab-case`, with the instructions in `SKILL.md`
- Examples:
  - `backend-api-agent.md`
  - `frontend-refactor-agent.md`
  - `skills/feature-review/SKILL.md`

## Prompt Lifecycle

1. Draft the prompt or skill with explicit constraints.
2. Run real tasks against it (at least 3 representative scenarios).
3. Record failure modes (ambiguity, inconsistency, missing edge cases).
4. Refine defaults, acceptance criteria, and response format.
5. Version changes with descriptive commit messages.
6. For a skill, copy the updated folder into `~/.cursor/skills/` before the next real task.

## Quality Gates

Before considering a prompt stable:

- Response format is consistent across multiple runs.
- Clarification questions are reduced to architecture-critical decisions.
- Output includes runnable validation steps.
- Safety and quality concerns are explicitly addressed when relevant.

## Contribution Workflow

1. Add or update files under `prompts/` or `skills/`.
2. Keep changes focused and documented.
3. When a skill changes, refresh the copy in `~/.cursor/skills/`.
4. Update this README when conventions evolve.
5. Open a PR whose body follows `prompts/pull_request_template.md`.

## Suggested Commit Types

- `feat(prompt): add new prompt`
- `feat(skill): add new skill`
- `refactor(prompt): improve prompt determinism`
- `docs: update repository standards`

## Roadmap

- Add a reusable prompt template (`prompts/_template.md`).
- Add domain-specific prompts (backend, frontend, debugging).
- Add benchmark tasks for regression testing prompt quality.