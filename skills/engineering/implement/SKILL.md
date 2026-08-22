---
name: implement
description: Implement work described by a spec and its linked issue-tracker tickets. Reads the spec for context and works through tickets in dependency order until all are done.
disable-model-invocation: true
---

# Implement

Implement the work described by a **spec** and its linked **issue-tracker tickets**.

## Prerequisites

This skill requires both:

1. A spec in `docs/spec/<NNNN>-<feature-slug>.md`
2. Corresponding tickets in `docs/issues-tracker/<NNNN>-<feature-slug>/`

If either is missing, tell the user and suggest:

- **No spec?** → "Use `/to-spec` to generate one from the current conversation."
- **No tickets?** → "Use `/to-tickets` to break the spec into implementable tickets."

Do not proceed without both artifacts.

## File structure

```
/
├── docs/
│   ├── spec/
│   │   └── 0001-event-sourced-orders.md        ← read this for context
│   └── issues-tracker/
│       └── 0001-event-sourced-orders/           ← work through these
│           ├── 0001-issue-title-1
│           ├── 0002-issue-title-2
│           └── 0003-issue-title-3
└── src/                                         ← write code here
```

## Process

### 1. Read the spec

Read the full spec to understand the problem, solution, user stories, implementation decisions, and testing decisions. This is your source of truth for what the feature should do.

### 2. Read all tickets

Read every ticket in the feature's `docs/issues-tracker/<NNNN>-<feature-slug>/` directory. Understand the dependency graph (blocked-by edges) so you can work in the right order.

### 3. Write gates BEFORE implementation (/unlazy)

Invoke `/unlazy` discipline upfront. Write `docs/issues-tracker/<NNNN>-<feature-slug>/GATES.md` with one gate per acceptance criterion across all tickets. This is your completion ledger for the entire feature — it exists before a single line of code is written.

The gates file tracks what "done" means. You do not report done until every gate has evidence.

### 4. Work the frontier

Implement tickets in dependency order — start with tickets that have no blockers, then move to tickets whose blockers are all complete.

For each ticket:

1. Read its acceptance criteria carefully.
2. Implement the work in `src/`, respecting the project's existing patterns and conventions.
3. Run typechecking after meaningful changes.
4. Run relevant tests regularly (single files as you go, full suite when the ticket is done).
5. Check off the corresponding gates in `GATES.md` with evidence as each criterion is provably met.
6. Mark the ticket status as complete once all its gates pass.

### 5. Final verification

- Run the full test suite.
- Run typechecking across the project.
- Verify no regressions in existing functionality.
- Re-read `GATES.md` — every gate must have evidence. Any gate still reading `pending` means the feature is not done.

### 6. Commit

Commit your work to the current branch. Use clear commit messages that reference the ticket numbers.

## Rules

- **No skipping tickets.** Every ticket in the feature directory must be completed unless explicitly abandoned with a reason.
- **No silent scope reduction.** If a ticket's acceptance criterion cannot be met, surface it to the user rather than quietly dropping it.
- **Respect the spec's testing decisions.** Use the testing approach described in the spec (seams, test types, prior art).
- **Match project conventions.** Read existing code before writing new code. Use the same libraries, patterns, and style.
