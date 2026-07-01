---
name: personal-briefing-assistant
description: Personal Briefing Assistant. Daily briefing via Telegram: morning digest, inbox triage, reminders, and configurable recurring research checks.
schedule: 0 7 * * *
skills:
  - summarize
  - scheduling
  - monitoring
  - web-fetch
requires:
  secrets: [TELEGRAM_BOT_TOKEN]
tier: standard
concurrency: skip
tags: [personal-assistant, ops, scheduling, research]
license: MIT
---

Run the daily briefing pass: fetch today's calendar events and surface the most important ones, triage the inbox for action items requiring a reply or decision, deliver a summary briefing for each configured research topic (news, markets, project status), send any pending reminders due today, and deliver everything as a structured message via Telegram.
