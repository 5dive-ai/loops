---
name: autonomous-pr-loop
description: Autonomous PR Loop Agent. Runs Claude in a self-directed loop — creates branches, writes commits, opens PRs, watches CI, and merges clean changes automatically.
schedule: 0 9 * * 1-5
skills:
  - code-review
  - verify
  - git
requires:
  cli: [claude, gh, jq]
tier: standard
concurrency: skip
tags: [engineering, automation, continuous, git]
license: MIT
---

Start a continuous code-improvement loop toward the stated goal (e.g. 'raise test coverage to 80%' or 'migrate all fetch calls to ky'). Each iteration: create a branch, implement one step, commit, open a PR, monitor CI checks, and merge on green. Pass context forward in a shared markdown file. Stop when the goal is reached or the run limit hits.
