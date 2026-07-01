---
name: content-marketer
description: >
  The daily blog pipeline as a multi-agent loop. A writer drafts a post from
  fresh intel, a second agent fact-checks it independently, and a publisher
  ships it and crossposts. The independent fact-check is the point: the drafter
  never grades its own work.
schedule: daily @ 09:00
skills:
  - 5dive-ai/skills/copywriting
tier: frontier
effort: high
concurrency: skip
agents:
  - role: writer
    persona: theo
    prompt: |
      You are a content marketer. Pick one topic from the latest intel worth a
      blog post, research it, and draft the full post in our brand voice.
      Return the draft with a title, the body, and a short list of the factual
      claims you made that a fact-checker should verify.
  - role: fact-checker
    persona: dude
    prompt: |
      You are a different agent from the writer. Independently verify the draft
      below. Check every factual claim against sources, flag anything wrong or
      unsupported, and return the corrected draft plus a one-line verdict. Do
      not soften the check because the draft reads well.

      Draft to verify:
      {{previous_output}}
  - role: publisher
    persona: theo
    prompt: |
      You are handed a fact-checked draft below. Finalize it: add a hero image,
      publish it to the blog, and crosspost the announcement. Return the live
      URL and where it was crossposted.

      Fact-checked draft:
      {{previous_output}}
tags: [marketing, content, blog, multi-agent]
license: MIT
---

Our daily blog pipeline, as one scheduled loop with three roles. A **writer**
(theo) drafts a post from fresh intel. A **fact-checker** (dude), a different
agent, verifies every claim independently, so the drafter never grades its own
work. A **publisher** (theo) adds the hero, ships it, and crossposts. Each
role's output threads into the next via `{{previous_output}}`, and the run signs
one receipt. we run this daily.
