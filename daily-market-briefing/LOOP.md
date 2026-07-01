---
name: daily-market-briefing
description: Daily Stock Market Analyst. Weekday market close: pulls multi-market prices, news and capital flows; LLM generates buy/sell signals; pushes to Slack or Telegram.
schedule: 0 10 * * 1-5
skills:
  - market-research
  - data-analysis
  - report-writing
  - web-fetch
requires:
  cli: [python]
  secrets: [SLACK_WEBHOOK_URL]
tier: standard
concurrency: skip
tags: [finance, research, data, ops]
license: MIT
---

On each trading day, fetch price data, news headlines, and capital-flow indicators for the configured watchlist (A/H/US/JP/KR or custom). Run LLM analysis across price action, news sentiment, and technical signals; generate buy/hold/sell recommendations with confidence levels; check holiday calendars to skip non-trading days; then push a formatted dashboard to the configured notification channel.
