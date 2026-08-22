---
name: walkthrough
description: Write a tutorial.md that walks a user through verifying agent-produced implementation work.
disable-model-invocation: true
---

# Walkthrough

Produce a `tutorial.md` that guides a human through verifying the implementation the agent just completed — every change accounted for, every verification step _actionable_.

## Determine scope

Two branches:

- **Post-implement** — a spec and tickets exist under `docs/`. Read the spec, all tickets, and `GATES.md`. The walkthrough mirrors the gates: one section per gate, in dependency order.
- **General execution** — no spec/ticket structure. Read the git diff (or recent commits) to discover what changed. Group changes by concern (new module, modified API, config change, etc.).

Done when: the set of things to verify is written down and nothing changed is missing from it.

## Write `tutorial.md`

Place the file so it mirrors the code structure it verifies:

- **Post-implement** — write to `docs/issues-tracker/<NNNN>-<feature-slug>/tutorial.md`, beside the tickets it covers.
- **General execution** — write to `docs/tutorial/<NNNN>-<feature-slug>.md`. Use the next available number and a short descriptive slug for the feature (e.g. `001-auth-flow.md`, `002-payment-webhook.md`).

Structure:

```
# Verification Walkthrough

## Prerequisites
<what the user needs running: dev server, database, env vars, installed deps>

## 1. <First verifiable unit>
**What changed:** <one-sentence summary>
**Files:** <paths to the changed files relevant to this unit>
**How to verify:**
1. <concrete action — a command, a URL to visit, a file to inspect>
2. <expected observation — what "working" looks like>

## 2. <Next unit>
...

## Regression check
<commands or steps that confirm nothing previously working is broken>
```

Rules for each section:

- **Concrete over conceptual.** Every "How to verify" must be a shell command, a URL, or a UI action — never "ensure it works."
- **Expected output.** State what the user should see, so they can diff reality against expectation.
- **Dependency order.** Earlier sections must pass before later ones make sense (e.g. "database migrated" before "API returns new field").
- **No assumed knowledge.** If a step requires a running service, say how to start it. If it needs test data, say how to seed it.

Done when: every item from the scope has a section, every section has at least one runnable verification command or observable action, and the prerequisite section accounts for all setup.

## Self-check

Re-read the finished `tutorial.md` against the scope list. Any scope item without a corresponding section is a gap — fill it. Any section whose "How to verify" cannot be executed without information not in the document is incomplete — add the missing setup.

Done when: a cold reader with access to the repo and the tutorial can verify every change without asking questions.
