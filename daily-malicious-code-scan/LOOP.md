---
name: daily-malicious-code-scan
description: Daily Malicious Code Scanner. Scans the last 3 days of code changes daily for supply-chain and malicious patterns, raising code-scanning alerts.
schedule: daily
skills:
  - security
  - code-review
  - github
requires:
  cli: [gh]
  secrets: [GITHUB_TOKEN]
tier: standard
concurrency: skip
tags: [security, dev, github]
license: MIT
---

Daily, analyze code changes from the last 3 days for suspicious patterns indicating malicious activity or supply-chain compromise, and create code-scanning alerts for anything suspicious.
