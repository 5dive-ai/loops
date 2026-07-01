---
name: affiliate-manager
description: >
  Runs an affiliate program end to end. Tracks referrals and conversions,
  calculates payouts, flags anomalies, and sends each affiliate their weekly
  numbers.
schedule: 0 9 * * 1
requires:
  secrets: [STRIPE_API_KEY]
tier: standard
effort: medium
concurrency: skip
tags: [marketing, affiliate, revenue, ops]
license: MIT
---

Each week, pull referral and conversion data, calculate what each affiliate is
owed, and flag anything that looks off such as self-referrals, refunds, or
spikes. Send every affiliate a short summary of their numbers and payout, and
keep a running ledger so nothing is double-counted.
