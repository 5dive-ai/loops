# Agentic Loops — spec + registry

The open standard and registry behind **[agenticloops.dev](https://agenticloops.dev?utm_source=github&utm_medium=referral&utm_campaign=loops-readme)**.

An **agentic loop** is an installable, recurring AI agent defined in a single file: a trigger, a set of skills, and a prompt. It runs on a schedule and completes a whole job on any agent harness.

## What's here
- **[`SPEC.md`](./SPEC.md)** — the `LOOP.md` standard (v0.1). Harness-agnostic, model-agnostic. One file defines a loop.
- **`schema.json`** — JSON Schema for registry entries.
- **`index.json`** — the curated registry consumed by the agenticloops.dev directory.

## Publish a loop
1. Put a valid `LOOP.md` in a public repo.
2. Add the GitHub topic **`agenticloops`**.
3. The crawler discovers and indexes it automatically — no PR needed.

Install any loop:

```bash
npx agenticloops install <owner/loop>
```

MIT.
