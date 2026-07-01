---
name: issue-triage-bot
description: Issue Triage Bot. Classifies every new GitHub issue by type and priority, asks clarifying questions, labels and routes it, and closes user-error reports with a clear explanation.
event: github-issue-opened
skills:
  - github
  - classify
  - draft-reply
requires:
  cli: [gh]
  secrets: [GITHUB_TOKEN]
tier: standard
concurrency: skip
tags: [community, github, automation, support]
license: MIT
---

A new GitHub issue was opened. Classify it as: bug, feature request, question, or user error. Apply the appropriate label and priority. If it's a bug, confirm reproduction steps and escalate to the engineering team. If it's a question or user error, draft a helpful reply explaining the correct behavior and close it. If it's a feature request, acknowledge it and add to the backlog label. Always respond within the issue thread.
