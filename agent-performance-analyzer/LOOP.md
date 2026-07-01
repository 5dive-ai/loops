---
name: agent-performance-analyzer
description: Agent Performance Analyzer. Daily meta-agent that analyzes the quality and effectiveness of your other AI workflows across the repo.
schedule: daily
skills:
  - analysis
  - evaluation
  - github
requires:
  cli: [gh]
  secrets: [GITHUB_TOKEN]
tier: standard
concurrency: skip
tags: [ops, ai-ops, github]
license: MIT
---

Daily, analyze AI agent performance, quality, and effectiveness across the repository's agentic workflows, summarizing where agents are helping and where they are underperforming.
