---
name: agentic-security-scanner
description: Agentic Workflow Security Scanner. Weekly scan of all AI agent configs for OWASP LLM Top 10 vulnerabilities, prompt injection surfaces, and PII leakage.
schedule: 0 3 * * 1
skills:
  - security
  - code-review
  - static-analysis
requires:
  cli: [python]
tier: standard
concurrency: skip
tags: [security, llm-safety, scanning, ai-ops]
license: Apache-2.0
---

Scan all agentic workflow configurations, LLM system prompts, and MCP server definitions in this repository. Map the agent architecture and tool-call graph, detect prompt injection attack surfaces, identify PII leakage risks in data flows, and score every finding against the OWASP LLM Top 10 and Agentic AI threat matrix. Output a structured report with severity, location, and remediation guidance.
