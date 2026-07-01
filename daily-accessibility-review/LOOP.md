---
name: daily-accessibility-review
description: Daily Accessibility Reviewer. Runs Playwright against your web app daily and reports WCAG 2.2 accessibility issues with fixes.
schedule: daily
skills:
  - accessibility
  - browser-automation
  - github
requires:
  cli: [gh]
  secrets: [GITHUB_TOKEN]
  mcp: [playwright]
tier: standard
concurrency: skip
tags: [dev, accessibility, qa]
license: MIT
---

Daily, use Playwright browser automation to review the web application against WCAG 2.2 guidelines, then create a discussion with identified accessibility issues and remediation recommendations.
