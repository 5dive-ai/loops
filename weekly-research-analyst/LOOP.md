---
name: weekly-research-analyst
description: Weekly Research & Competitive Analyst. Every Monday, researches industry news, related products and papers, then posts a competitive-analysis discussion.
schedule: 0 9 * * 1
skills:
  - deep-research
  - web-fetch
  - summarize
  - github
requires:
  cli: [gh]
  secrets: [GITHUB_TOKEN]
tier: standard
concurrency: skip
tags: [research, competitive-intel, github]
license: MIT
---

Weekly, review recent code, issues and PRs plus industry news and trends, and create a GitHub discussion with research findings covering related products, papers, market opportunities, and new ideas.
