---
name: ci-failure-doctor
description: CI Failure Doctor. Triggers whenever a monitored CI workflow fails and posts a root-cause analysis with remediation steps.
event: workflow_run.completed (failure)
skills:
  - github
  - debugging
  - log-analysis
requires:
  cli: [gh]
  secrets: [GITHUB_TOKEN]
tier: standard
concurrency: skip
tags: [dev, ci, github]
license: MIT
---

When a monitored workflow fails, perform deep analysis of the GitHub Actions failure: examine logs, error messages, and workflow configuration to identify root causes and provide actionable remediation steps.
