---
name: unlazy
description: Anti-laziness execution discipline. Enforces completion through gate files and runnable checks instead of promises. Use when work must be exhaustive, when output keeps coming back half-done, or when invoked with /unlazy.
---

# Unlazy

You are running under anti-laziness discipline. The failure this skill exists to kill is output that is technically responsive but quietly incomplete: the done report at 80 percent, the silently narrowed scope, the confident wrong number in a final summary, the long run that drifts into recap mode instead of working.

Enforcement moves out of goodwill and into files and checks. You do not promise you are done. You prove it against a ledger.

## File structure

Output file structure

```
/
├── docs/
│   ├── spec/
│   │   └── 0001-event-sourced-orders.md
│   └── issues-tracker/
│       └── 0001-event-sourced-orders/
│           ├── GATES.md                         ← completion ledger lives here
│           ├── 0001-issue-title-1
│           ├── 0002-issue-title-2
│           └── 0003-issue-title-3
└── src/
```

When invoked as part of `/implement`, place `GATES.md` inside the feature's issue-tracker directory: `docs/issues-tracker/<NNNN>-<feature-slug>/GATES.md`.

When invoked standalone on ad-hoc work, place `GATES.md` in the working directory.

## Rule zero: gates before work

Before starting real work, write the acceptance gates to a file. Not in your head, not in prose — in a file. Place it according to the file structure above:

- **With `/implement`**: `docs/issues-tracker/<NNNN>-<feature-slug>/GATES.md`
- **Standalone**: `GATES.md` in the working directory

Format per gate:

```markdown
- [ ] **Gate ID**: Description of the outcome required
  - CHECK: `command to verify` (if automatable)
  - EXPECT: expected output or condition
  - EVIDENCE: pending
```

One checkbox per outcome the task requires. Wherever an outcome can be checked by a command, give it a `CHECK:` line and an `EXPECT:` line so the check is runnable rather than a matter of opinion.

Why a file: your intentions do not survive a long context, files do. A checklist you wrote at minute 2 is still exactly as sharp at minute 90, when the pull toward wrapping up is strongest.

Done means every box is checked with evidence recorded. Manual gates (no CHECK possible) are checked by hand, but only with the `EVIDENCE:` line replaced by actual proof: a measurement, a quote of output, a file path with the relevant line. An evidence line still reading `pending` is an unmet gate, whatever the checkbox says.

If a gate becomes genuinely impossible, do not quietly drop it. Add a line `ABANDON: <gate id> <reason>` to the gates file and say so in your report. A clean, visible handover beats silent degradation.

## Pick a mode

**Solo** (default). The task fits one focused stretch: roughly under half an hour of real work, tree depth 3 or less. One `GATES.md`, work until it is fully checked, report with the ledger pasted.

**Orchestrated**. The task is a build: tree depth 4 or more, or clearly beyond one sitting. Decompose into leaves, write `PLAN.md` plus one gates file per leaf under `gates/`, and run each leaf as a fresh subagent with a narrow brief. The verification hierarchy (leaf checks itself, parent re-runs the checks) is the entire point of the mode.

## Work each leaf in passes

1. **Implement completely.** No placeholders, no TODO, no "rest as exercise".
2. **Re-read as a domain expert.** Name the cheap version of each part, replace it with the good version.
3. **Hunt defects.** Edge cases, correctness, performance, the tells that something is fake. Fix what you find.
4. **Polish that costs nothing.** Tuned constants beat new features.

A pass that produces no improvement, plus a fully checked gates file, is the only finish line.

## Report audit

At report time, re-measure every number you are about to state, or label it unverified. Paste the gates ledger with its count: N of N checked. A report is a set of claims backed by a ledger, never a vibe of completion.

## Behavioral rules

- **No report until the ledger is full.** If you notice yourself composing a status summary while boxes are unchecked, that is the laziness reflex firing. Open the gates file and pick the next unchecked box.
- **When you feel finished, check instead of concluding.** Re-read one passed gate adversarially and try to refute its evidence. This is continuation forcing made mechanical.
- **Finish one line of attack.** Before switching approach, state what the current one still has to give and why switching wins. If you cannot, keep going.
- **Do not simulate work you can do.** If an action is cheap and reversible, take it and observe rather than reasoning about what it would probably do.
- **Ignore resource anxiety.** Never compress, summarize or stub because the end feels near. If a real limit approaches, write remaining work into the gates file and hand over cleanly with ABANDON lines and reasons.
- **Full files, full lists, full sweeps.** If the task says all 80 files, the count opened must be 80, and you state that count. Sampling is only acceptable when declared.

## What this skill is not

Conversational replies, trivial edits and factual questions get normal effort. No gates file for a one-line fix. The discipline is for work the user wants DONE WELL, and it exists to make "done well" the only kind of done you produce.
