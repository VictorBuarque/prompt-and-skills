---
name: pr-markdown
description: >-
  Always writes pull request title and body to a PR.md file before (or instead
  of) opening a GitHub PR. The body carries the feature-review author proof:
  four safety anchors, test results, runtime path, screenshot or short video,
  what stayed unmodified, and confidence. Use when the user asks for PR text,
  PR description, texto de PR, create PR, open PR, gh pr create, or any
  pull-request draft.
---

# PR Markdown

Whenever a pull request description is needed, **always** materialize it as a markdown file first. Do not only paste the PR body in chat.

The file is the author's proof for feature review. It is not the review verdict. Do not write Merge-ready or Launch-ready here.

Read the `feature-review` skill and use its names for the anchors, the proofs, and the experiment.

## Output file

1. Write (or overwrite) **`PR.md`** at the **repository root**.
2. Do **not** commit `PR.md` unless the user explicitly asks.
3. If the user also asks to open the PR on GitHub, use the contents of `PR.md` as the `gh pr create` body (after pushing if needed). Prefer reading `PR.md` rather than regenerating from memory.

## Gather context first

Run in parallel against the repo base branch (usually `main`):

- `git status`
- `git diff` / `git diff <base>...HEAD`
- `git log <base>..HEAD --oneline`
- Branch tracking vs remote (if creating the remote PR)

Base the summary on **all commits** on the branch, not only the latest.

From that diff, separate core infrastructure, shared behavior, and isolated leaves the way `feature-review` defines them. Find the flag, the off behavior, and the rollback. Use only runs you actually executed for Tests, Runtime, and Visual.

## `PR.md` template

```markdown
# <PR title — concise, imperative or descriptive>

## Summary

- <1–3 bullets: why this exists and what the user can do>

## O que mudou? (What changed)

<behavior, not the file list>

## O que não deve ser mudado? (What must not change)

<invariants and adjacent flows>

## Até onde o fracasso pode ir? (How far can failure travel?)

<users, data, money, permissions, trunk>

## Podemos bloquear ou reverter isso? (Can we gate or roll it back?)

<flag, off behavior, rollback. A flag is a gate only when off restores current behavior without a deploy.>

## Proof

- Tests: <command and result, or Missing>
- Runtime path: <path exercised end to end, or Missing>
- Visual: <screenshot or short video, or Not applicable, or Missing>
- Left unmodified: <what stayed the same, and how that was checked>

## Confidence

- Verified:
- Not verified:

## Experiment

1. Gate:
2. Start small:
3. Watch:
4. Decide:

## Test plan

- [ ] <step a reviewer can repeat>
- [ ] <…>
```

## How to fill it

- Title on line 1 as `# …` (also usable as `gh pr create --title`).
- Summary: intent and user-visible impact. No file dump.
- Keep the four anchor headings exactly as written.
- Proof is what already ran. Test plan is what the reviewer repeats. Do not paste the same unchecked list into both.
- Tests name the command and the result. Runtime names the path. Visual is required when a user will see the change. A still image does not replace the runtime path.
- Anything not run is `Missing` or `Not verified`. Do not invent a pass.
- If nobody receives the feature, Experiment is one line: `Not a launch. Flag stays off.`
- If anyone receives it, fill all four experiment steps. Name the limited group, the failures, the product signals, the expand rule, and the rollback rule.
- Match the conversation / project language (pt-BR for this team unless asked otherwise).
- Keep the file self-contained; no “as discussed” without the facts.

## After writing

Tell the user the path (`PR.md`) briefly. Only run `gh pr create` when they asked to open/create the PR on GitHub.
