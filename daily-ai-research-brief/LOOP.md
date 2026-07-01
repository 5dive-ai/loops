---
name: daily-ai-research-brief
description: Multi-Agent AI Research Brief. Spawns parallel research agents across 9 AI topic areas, synthesizes into a TL;DR + full report, and pushes to Notion or Slack.
schedule: daily
skills:
  - deep-research
  - summarize
  - report-writing
requires:
  secrets: [ANTHROPIC_API_KEY, NOTION_API_KEY]
tier: standard
concurrency: skip
tags: [research, ai-news, content, multi-agent]
license: MIT
---

Spawn parallel deep-research agents, one per AI topic area: models, safety, agents, tools, applications, policy, infrastructure, research papers, and industry news. Each agent researches its domain independently. Once all complete, synthesize findings into a TL;DR executive summary (5 bullets max) and a full structured report with sources. Publish to the configured destination (Notion, Obsidian, Slack, or Teams).
