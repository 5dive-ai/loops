---
name: daily-ai-news-digest
description: Daily AI News Digest Writer. Every morning, fetches AI industry news, categorizes and summarizes by topic, and generates styled summaries and social sharing cards.
schedule: daily
skills:
  - web-fetch
  - summarize
  - content-generation
requires:
  cli: [gh]
  secrets: [GITHUB_TOKEN, ANTHROPIC_API_KEY]
tier: standard
concurrency: skip
tags: [content, research, newsletter, ai-news]
license: MIT
---

Each morning, fetch recent AI industry news from configured sources, categorize each item by topic (models, tools, research, products, policy), summarize in a clear consistent format, generate a structured Markdown digest with multiple display themes, and produce social-platform sharing cards formatted for major platforms.
