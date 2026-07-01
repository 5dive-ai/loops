---
name: daily-news-radar
description: Daily Personal News Radar. Aggregates HN, RSS, Reddit, GitHub releases and financial feeds; AI-scores each item; delivers a daily briefing to email or Slack.
schedule: daily
skills:
  - news-aggregation
  - web-fetch
  - summarize
  - content-generation
requires:
  cli: [python]
  secrets: [EMAIL_SMTP_PASS]
tier: standard
concurrency: skip
tags: [research, content, personal-assistant, newsletter]
license: MIT
---

Each morning, aggregate content from configured sources (HN, RSS feeds, Reddit, GitHub releases, financial data). Score each item 0-10 for relevance to configured interest topics, deduplicate across sources, enrich high-scoring items with additional web context, then generate a concise daily briefing and deliver it to the configured channel (email, Slack, Discord, or Feishu).
