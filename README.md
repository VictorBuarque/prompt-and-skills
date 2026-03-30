# Copilot Prompts Repository

Centralized repository for authoring, versioning, and evolving AI agent prompts with engineering-grade standards.

## Overview

This repository is designed to make prompt development deterministic, reusable, and maintainable across projects.  
It focuses on implementation-oriented prompts that drive real code changes, not generic brainstorming output.

## Goals

- Establish a single source of truth for prompt assets.
- Improve prompt quality over time through versioned iterations.
- Standardize prompt structure and response expectations.
- Enable cross-project and cross-team reuse.

## Repository Structure

```text
.
├── prompts/
│   ├── prompt-agent.md
│   ├── prompt-ask.md
│   ├── prompt-plan.md
│   └── prompt-study.md
└── README.md
```

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

- File naming: `kebab-case.md`
- Examples:
  - `backend-api-agent.md`
  - `frontend-refactor-agent.md`
  - `code-review-agent.md`

## Prompt Lifecycle

1. Draft prompt with explicit constraints.
2. Run real tasks against it (at least 3 representative scenarios).
3. Record failure modes (ambiguity, inconsistency, missing edge cases).
4. Refine defaults, acceptance criteria, and response format.
5. Version changes with descriptive commit messages.

## Quality Gates

Before considering a prompt stable:

- Response format is consistent across multiple runs.
- Clarification questions are reduced to architecture-critical decisions.
- Output includes runnable validation steps.
- Safety and quality concerns are explicitly addressed when relevant.

## Contribution Workflow

1. Add or update prompt files under `prompts/`.
2. Keep changes focused and documented.
3. Update this README when conventions evolve.
4. Open a PR with:
   - **Summary**
   - **Technical Changes**
   - **Validation Notes**
   - **Risks / Trade-offs**

## Suggested Commit Types

- `feat(prompt): add new prompt`
- `refactor(prompt): improve prompt determinism`
- `docs: update repository standards`

## Roadmap

- Add a reusable prompt template (`prompts/_template.md`).
- Add domain-specific prompts (backend, frontend, review, debugging).
- Add benchmark tasks for regression testing prompt quality.