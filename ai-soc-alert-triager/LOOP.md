---
name: ai-soc-alert-triager
description: AI Security Operations Center. Nightly SOC loop: fuses security alerts, maps to MITRE ATT&CK, triages by severity, and runs purple-team drills.
schedule: 0 2 * * *
skills:
  - security
  - threat-intelligence
  - log-analysis
  - monitoring
requires:
  cli: [python]
  secrets: [SIEM_API_KEY]
tier: standard
concurrency: skip
tags: [security, soc, ops, threat-intel]
license: MIT
---

Each night, collect and fuse security alerts from SIEM and log sources, investigate findings and map each to MITRE ATT&CK tactics and techniques, triage by severity, generate Hunt-as-Code hypotheses for high-severity findings, and run any scheduled purple-team simulation drills configured in the hunt schedule.
