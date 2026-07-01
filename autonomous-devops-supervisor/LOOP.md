---
name: autonomous-devops-supervisor
description: Autonomous DevOps Supervisor. Checks every 2 minutes: auto-merges ready PRs, diagnoses failing ones, detects stuck work, and kills runaway agents.
schedule: every 2 minutes
skills:
  - github
  - code-review
  - log-analysis
  - monitoring
requires:
  cli: [gh, node]
  secrets: [GITHUB_TOKEN]
tier: standard
concurrency: skip
tags: [dev, ops, autonomous, ci]
license: MIT
---

Check the repository every 2 minutes: auto-merge PRs that pass all CI checks and are marked ready, post diagnostic comments on failing PRs, flag any workflow run stuck for 6+ hours, advance multi-day project milestones, sync open TODO comments to GitHub issues, and terminate stuck agent processes.
