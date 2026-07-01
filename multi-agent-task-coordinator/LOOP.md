---
name: multi-agent-task-coordinator
description: Multi-Agent Task Coordinator. Receives tasks from Slack, GitHub, or email; delegates to isolated worker agents; compounds shared memory over time.
event: slack-message | github-issue | email
skills:
  - task-delegation
  - planning
  - summarize
  - monitoring
requires:
  cli: [docker]
  secrets: [SLACK_BOT_TOKEN, GITHUB_TOKEN]
tier: standard
concurrency: skip
tags: [ops, multi-agent, coordination, automation]
license: MIT
---

Monitor inbound task channels (Slack, GitHub issues, email). When a task arrives, analyze scope, spawn isolated Docker worker agents for each sub-task, track progress via shared persistent memory that compounds context over time, and post a summary to the originating channel when complete.
