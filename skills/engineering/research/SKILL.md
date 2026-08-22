---
name: research
description: Investigate a question against high-trust primary sources and capture the findings as a Markdown file in the repo. Use when the user wants a topic researched, docs or API facts gathered, or reading legwork delegated to a background agent.
---

## File structure

Output file structure

```
/
├── docs/
│   └── research/
│       ├── 0001-topic-slug.md
│       └── 0002-another-topic.md
└── src/
```

Create files lazily — only when you have something to write. If no `docs/research/` exists, create it when the first research note is needed.

## Process

Spin up a **background agent** to do the research, so you keep working while it reads.

Its job:

1. Investigate the question against **primary sources** (official docs, source code, specs, first-party APIs), not a secondary write-up of them. Follow every claim back to the source that owns it.
2. Write the findings to a single Markdown file under `docs/research/<NNNN>-<topic-slug>.md`, citing each claim's source.
3. Number sequentially from the last existing file in `docs/research/`.