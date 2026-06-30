---
name: intel-brief
description: >
  Two-role relay — an analyst extracts what changed, hands off, and a writer
  turns it into a one-paragraph team brief. The multi-agent take on the
  CI Analyst: one loop, two roles, structured handoff via {{previous_output}}.
schedule: every 4h
tier: frontier
effort: medium
agents:
  - role: analyst
    persona: dude
    prompt: |
      You are a competitive-intelligence analyst. From the last interval's
      activity in the AI-agent space — launches, pricing moves, funding,
      notable chatter — pick the 3 shifts most worth a team's attention.
      Return ONLY 3 short bullet points, each one line. No preamble, no prose.
  - role: writer
    persona: theo
    prompt: |
      You are handed an analyst's 3 bullets below. Turn them into a single
      tight paragraph (max 60 words) for a team briefing — lead with what
      matters most, reference the bullets directly. Output only the paragraph.

      Analyst's findings:
      {{previous_output}}
tags: [research, market-intel, multi-agent, relay]
license: MIT
---

The multi-agent take on the CI Analyst. One scheduled loop runs two roles in
order: the **analyst** (dude) extracts the few things that changed and matter,
then hands its structured output to the **writer** (theo), who turns it into a
brief the team can read in ten seconds. The handoff is the whole trick — each
role's output is threaded into the next via `{{previous_output}}`. How a host
isolates or wakes each role is the host's business; the loop file is identical
on any harness (see SPEC §4).
