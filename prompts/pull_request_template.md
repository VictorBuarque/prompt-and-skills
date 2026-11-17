<!--
Author proof for feature review. Do not write Merge-ready or Launch-ready.
Proof is what already ran. Test plan is what the reviewer repeats.
Anything not run is Missing or Not verified.
If nobody receives the feature, replace Experiment with: Not a launch. Flag stays off.
-->

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
