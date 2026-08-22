---
name: grill-me-auto
description: Predicts decisions a grilling session will require, captures the user's decision principles upfront, then runs the grill answering on user behalf.
disable-model-invocation: true
---

# Auto-Grill

The grilling skill explores a design tree — architecture, tool choices, service selection, trade-offs. Most decisions in that tree share a small set of driving principles the user already holds but hasn't articulated. This skill extracts those principles *before* the grill runs deep, then uses them to answer on the user's behalf.

## Step 1 — Predict the decision landscape

Given the user's topic (a feature, system, or design problem):

1. Analyse the topic and predict the categories of decisions a grilling session will surface. Common categories: architecture style, data ownership, performance vs simplicity, build vs buy, coupling boundaries, operational cost appetite, team capability constraints.
2. For each category, draft 1–2 **principle questions** — questions whose answers reveal the user's standing preference, not a one-off choice. Examples:
   - "When forced to choose, do you optimise for operational simplicity or flexibility?"
   - "What's your default stance on third-party services vs self-hosted?"
   - "How do you weigh upfront build cost against long-term maintenance?"
3. Present the predicted categories and principle questions to the user. Ask them to answer each, skip irrelevant ones, or add categories you missed.

Completion criterion: user has answered (or explicitly skipped) every principle question, and confirmed the resulting **principles list**.

## Step 2 — Run the grill autonomously

Dispatch a sub-agent to run the full grilling session without user input. The sub-agent receives:
- The confirmed principles list
- The topic and any context gathered in Step 1

The sub-agent runs the design-tree and frontier mechanics from the `grilling` skill, but answers every question itself by reasoning from the principles. No user interaction. For each question it records:
- The question (verbatim, as a grill session would ask it)
- Its answer and which principle(s) drove it
- A **confidence** tag: `high` (one principle clearly decides), `medium` (principles align but don't directly address it), `low` (principles are in tension or silent — the sub-agent is guessing)

The sub-agent continues until the frontier is empty — every branch of the design tree visited.

Completion criterion: sub-agent returns with every frontier question answered and the design tree fully resolved.

## Step 3 — Generate the review file

Produce a markdown file at `docs/grill/<session-slug>.md` containing:

```markdown
# Grill Session: <topic>

Date: <ISO date>
Method: Auto-grill — principle-based delegation

## Principles

| # | Principle | Category |
|---|-----------|----------|
| 1 | ...       | ...      |

## Decisions

### High Confidence (principles clearly decided)

| # | Question | Answer | Driving Principle |
|---|----------|--------|-------------------|
| 1 | ...      | ...    | #N: ...           |

### Medium Confidence (principles align but don't directly address)

| # | Question | Answer | Driving Principle | Reasoning |
|---|----------|--------|-------------------|-----------|
| 1 | ...      | ...    | #N: ...           | ...       |

### Low Confidence (needs human review)

| # | Question | Answer | Reasoning | Tension |
|---|----------|--------|-----------|---------|
| 1 | ...      | ...    | ...       | ...     |

## Design Tree

<compact representation of the resolved tree>
```

Low-confidence answers are where the principles were silent or in conflict — these are the rows the human most needs to review. Call this out explicitly at the top of the file.

Completion criterion: file written to disk, path reported to user.
