---
name: feature-review
description: >-
  Reviews the whole feature instead of the diff. Classifies paths as core
  infrastructure (deep-read), shared behavior (careful), or isolated leaf code
  (proof-led). Requires safety anchors and PR proofs (test results, runtime
  path, screenshot or short video, what stayed unmodified), and separates
  merge-ready from launch-ready. Treats every launch as a gated experiment.
  Use when the user asks for a review, revisão, code review, PR review, feature
  review, launch review, lançamento, merge-ready or launch-ready check,
  post-launch validation, or when planning, gating, or proving a feature.
---

# Feature review

Review the feature. The diff is an input. Merge-ready is not launch-ready.

Control the code. Deep-read the trunk. Validate behavior. Attack the whole product before launch.

Write the report in the user's language. Keep the four anchor headings exactly as in the template.

A diff-only pass is evidence for the trunk read. It is not the verdict.

## Classify every touched path

Assign one class before reviewing the path.

| Class | Where | How |
| --- | --- | --- |
| Core infrastructure | Trunk: shared runtime, auth, data access, routing, flags, money, permissions | **Deep-read** |
| Shared behavior | Used by more than one feature or app | **Careful** |
| Isolated leaf | One feature, no shared contract | **Proof-led** |

**Deep-read.** Read the module, not the hunk. List callers. State the invariant the change must keep. Trace one failure (bad input, denied permission, dependency down) out of the module.

**Careful.** Read the change and the public contract. Check every caller that can observe it. Name the invariant that must hold.

**Proof-led.** Read enough to confirm the leaf is isolated and the proof matches the behavior. Then stop. No new shared export, no trunk write, flag off unchanged.

Never proof-lead core infrastructure.

## Author: build behind the flag

Use this order when planning or implementing. When reviewing, check that the change followed it. An accidental trunk edit is a finding.

### 01 Roadmap

Map the work completely before editing shared paths or the trunk:

- User flows added or changed
- Core infrastructure to touch
- Shared behavior to touch
- Isolated leaves
- Flag, and the behavior when the flag is off
- Rollback
- How far failure can travel

Do not edit the trunk in this step.

### 02 Broad strokes

Build isolated pieces behind the flag. Flag off keeps current behavior. Do not wire them into shared paths yet. Prove each piece with focused tests.

### 03 Integrate Later

Touch shared paths and the trunk on purpose, after the isolated pieces exist. Re-read those files at Careful or Deep-read depth. Answer the four anchors again after the integration.

## Proof the author owes

The author proves the change and puts the proof on the PR. The reviewer re-checks the proof. Do not accept a summary that cites a run nobody can find.

| Proof | What it must show |
| --- | --- |
| Tests | Focused logic and integration checks. Command and result. |
| Runtime | The feature exercised end to end. The path, named. |
| Visual | What the user will see, when the change is visible. |
| Confidence | What was verified and what was not. Gaps stay gaps. |

## Proofs that must be on the PR

A PR missing any required item is not reviewable. Name the gap. Do not approve from the diff.

- Test results
- Runtime path exercised
- Screenshot or short video, when a user will see the change
- What was left unmodified, and how that was checked

The author writes the four anchors, the Proof section, and Confidence into the PR description. The reviewer re-checks them and adds the verdict.

## When the change may move faster

A leaf may take the fast path only when all four are true:

1. Currently isolated
2. After the main feature
3. Visual or execution proof exists
4. Cheap to roll back

The fast path skips line-by-line reading. It does not skip the anchors or the proof. Core infrastructure and shared behavior never take the fast path.

## Four anchors

Answer these before any verdict. An unknown answer means the verdict is not ready.

1. **O que mudou? (What changed)** — the behavior, not the file list.
2. **O que não deve ser mudado? (What must not change)** — invariants and adjacent flows.
3. **Até onde o fracasso pode ir? (How far can failure travel?)** — users, data, money, permissions, shared trunk.
4. **Podemos bloquear ou reverter isso? (Can we gate or roll it back?)** — the flag, the off behavior, and the rollback step.

A flag is a gate when off restores current behavior without a deploy. A revert commit is a rollback, not a gate. Launch-ready needs the gate. A flag that only hides a broken trunk is not a gate.

## Review procedure

1. Map the feature the way 01 Roadmap describes, before judging lines.
2. Classify each touched path. Deep-read the trunk. Be careful on shared behavior. Follow the proof on isolated leaves.
3. Check the PR proofs. Re-run the focused tests when the repo can run them. Exercise the runtime path when the app or harness is available. For a visible change, look at the screenshot or video and confirm it matches the runtime path. A still image alone is not the runtime proof.
4. Answer the four anchors from the map and the run.
5. Give two verdicts: merge-ready and launch-ready.
6. Fill Confidence. Anything not run is Not verified.

### Merge-ready

All of these:

- Anchors answered
- Changed behavior has test results and a runtime path
- Visual proof when the user will see a change
- "What must not change" is proved, or failure cannot leave the leaf because the flag is off
- Shared and trunk edits were intentional and read at the right depth

### Launch-ready

Merge-ready, plus the experiment below, plus a product attack on every surface that shares trunk, state, permissions, or data with the feature.

Not verified on that attack means not launch-ready. Not verified on an untouched, unrelated surface does not block merge-ready.

## Launch is an experiment

1. **Gate it.** The feature stays easy to disable. Off restores current behavior.
2. **Start small.** Name the limited group. Do not open it to everyone first.
3. **Watch.** Name the failures and the product signals to track. Errors alone are not the watch list.
4. **Decide.** State the rule for expanding and the rule for rolling back.

A missing step means not launch-ready.

## Before launch and after launch

Before launch, attack the product surfaces that share trunk, state, permissions, or data with this feature. Review less of the leaf. Validate more of the behavior around it.

After launch, run four passes across the product. Use parallel agents when they are available. Give each pass the feature, the flag, the shared trunk, and one job. Each pass reports only what it executed and marks the rest Not verified.

| Pass | Job |
| --- | --- |
| Features | Run the full user flow. |
| Performance | Profile slow paths and bottlenecks. |
| Bugs | Explore bad states and failures. |
| Security | Test inputs, permissions, and data. |

## Report

Fill this template. Do not drop a section. Write Not verified or Missing instead of a guess.

```markdown
# Feature review — <feature>

## O que mudou? (What changed)

<behavior>

## O que não deve ser mudado? (What must not change)

<invariants and adjacent flows>

## Até onde o fracasso pode ir? (How far can failure travel?)

<users, data, money, permissions, trunk>

## Podemos bloquear ou reverter isso? (Can we gate or roll it back?)

<flag, off behavior, rollback>

## Reading

| Path | Class | How reviewed |
| --- | --- | --- |
| <path> | Core infrastructure / Shared behavior / Isolated leaf | Deep-read / Careful / Proof-led / Fast path |

## Proof

- Tests: <command and result, or Missing>
- Runtime path: <path exercised, or Missing>
- Visual: <screenshot or short video, or Not applicable, or Missing>
- Left unmodified: <what, and how it was checked>

## Verdict

- Merge-ready: Yes / No — <why>
- Launch-ready: Yes / No — <why>
- Fast path: Yes / No — <which conditions hold>

## Experiment

1. Gate:
2. Start small:
3. Watch:
4. Decide:

## Confidence

- Verified:
- Not verified:
```

## Rules

- Review less code. Validate more behavior.
- Do not approve while a required proof is Missing.
- Do not claim a run you did not do.
- Do not give a fast path to core infrastructure or shared behavior.
- Do not call the feature launch-ready because it is merge-ready.
