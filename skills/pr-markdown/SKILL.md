---
name: pr-markdown
description: >-
  Always writes pull request title and body to a PR.md file before (or instead
  of) opening a GitHub PR. Use when the user asks for PR text, PR description,
  texto de PR, create PR, open PR, gh pr create, or any pull-request draft.
---

# PR Markdown

Whenever a pull request description is needed, **always** materialize it as a markdown file first. Do not only paste the PR body in chat.

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

## `PR.md` template

```markdown
# <PR title — concise, imperative or descriptive>

## Summary

- <1–3 bullets: why / what changed for reviewers>

## Test plan

- [ ] <concrete verification step>
- [ ] <…>
```

Rules:

- Title on line 1 as `# …` (also usable as `gh pr create --title`).
- Summary: focused on intent and user-visible impact; avoid dumping file lists.
- Test plan: checkboxes with actionable steps (routes, redirects, UI states, CMS, mobile).
- Match the conversation / project language (pt-BR for this team unless asked otherwise).
- Keep the file self-contained; no “as discussed” without the facts.

## After writing

Tell the user the path (`PR.md`) briefly. Only run `gh pr create` when they asked to open/create the PR on GitHub.
