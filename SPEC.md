# The Agentic Loops Spec — `LOOP.md` (v0.1)

> An **agentic loop** is an installable, recurring AI agent defined in a single file: a trigger, a set of skills, and a prompt. It runs on a schedule and completes a whole job on any agent harness.
> One file defines it. Any harness can run it.

This is the open, harness-agnostic standard behind [agenticloops.dev](https://agenticloops.dev).
It is the loop-level analogue of [skills.sh](https://skills.sh)'s `SKILL.md`: a single
markdown file with YAML frontmatter that any compatible runtime can discover, validate, and install.

A "loop" here is **not** the inner reason→act→observe cycle of one agent (the ReAct loop).
It is the *outer* unit of distribution: a packaged, scheduled or event-triggered job you
install onto a runtime the way you install a skill or an app.

---

## 1. The file

A loop is a directory (or repo) containing a `LOOP.md`. The folder name is the loop id.

```markdown
---
name: ci-analyst
description: >
  Competitive Intelligence Analyst — watches every competitor and the field,
  catches what changed, and writes a digest before it matters.
schedule: every 4h
skills:                                  # owner/repo/skill — explicit & unambiguous
  - 5dive-ai/skills/deep-research
  - 5dive-ai/skills/compile-knowledge
requires:                       # what the loop needs in its environment
  cli: [gh]                     #   binaries that must be on PATH
  secrets: [X_API_TOKEN]        #   env-var NAMES only — never values
  mcp: [github]                 #   optional MCP servers
  network: [api.x.com]          #   optional egress allowlist
tier: frontier        # optional capability hint: frontier | standard | fast
effort: high          # optional reasoning budget: high | medium | low
concurrency: skip     # skip | queue | replace | allow  (overlap policy)
tags: [research, market-intel, competitive]
license: MIT
---

Scan our competitor set and the field for the last interval — launches, pricing,
funding, notable chatter. Update the watchlist and, once a day, write a concise
sourced briefing of what changed and what it means for us, then post it to the team.
```

The **frontmatter** is the manifest. The **body** is the starter prompt (the task, in
natural language). That's the whole format.

---

## 2. Fields

| Field | Required | Meaning |
|---|---|---|
| `name` | ✓ | kebab-case stable id (≤ 64 chars). Matches the folder name. |
| `description` | ✓ | What it does + when to use it. Drives discovery/search (like `SKILL.md`). |
| `schedule` | ✓* | Trigger time. Human grammar (`every 4h`, `hourly`, `daily @ 07:00`, `weekdays @ 09:00`) **or** raw cron. *Required unless `event` is set. |
| `event` | ✓* | Event trigger (`task-done`, `pr-opened`, `push`, a webhook id). Use instead of, or with, `schedule`. |
| `skills` | – | Array of skill names the loop needs. Resolved per §3.1 — bare names resolve against a default registry; `owner/skill` or `{id, source}` to be explicit. |
| `requires` | – | The loop's environment, for install-time pre-flight (§3.2). Sub-keys: `cli` (binaries on PATH), `secrets` (env-var **names** only — never values), `mcp` (MCP servers), `network` (egress allowlist). |
| `tier` | – | Optional **capability** hint: `frontier` \| `standard` \| `fast`. The harness maps it to its own model lineup. **Never name a vendor model** (`opus`/`gpt-5`/…) in a portable loop — that's a host-side install override only. |
| `effort` | – | Optional reasoning budget: `low` \| `medium` \| `high`. |
| `concurrency` | – | Overlap policy when a run is still going: `skip` (default) \| `queue` \| `replace` \| `allow`. |
| `persona` | – | Optional. A plain name/handle for the character that runs the loop. Cosmetic only; **no external spec required**. A host that supports persona cards (e.g. OpenAgent) may resolve it; everything else ignores it. |
| `agents` | – | Optional. An **ordered chain of roles** for a multi-agent loop (§4). When present, it replaces the single starter prompt — each role has its own prompt and the output of one role flows into the next. |
| `timezone` | – | IANA tz for clock-based schedules (default `UTC`). |
| `timeout` | – | Per-run wall-clock cap (`30m`, `1h30m`). |
| `tags` | – | Free-form labels for the directory's Topics. |
| `license` | – | SPDX id. |

Body (after frontmatter) = the **starter prompt**. Markdown. Multiple `#` task headings
are allowed for multi-step loops; each heading is a prompt run in order.

### Why these names
The field set is deliberately **interoperable with the conventions already in the wild**
(`schedule`, `skills`, `effort`, `concurrency`) so existing recurring-agent files can be
auto-ingested. The one intentional departure that keeps it truly agnostic: capability is a `tier`
hint, never a vendor model id (see §3). A loop needs nothing beyond this file — no
companion spec, no persona system — so a single `LOOP.md` is enough to adopt the standard.

---

## 3. Harness-agnostic — and model-agnostic — by design

A loop describes **intent**, not a runtime. `skills: [deep-research]` names a capability, not
a file path; `tier: frontier` names a quality bar, not a vendor SKU. A compatible harness is
anything that can resolve the named skills, honor the trigger, map the tier to one of its own
models, and run the prompt under the (optional) persona. That includes Claude Code, Cursor,
Copilot, Gemini CLI, Codex, and 5dive's own runtime.

**No vendor model names in a portable loop.** A loop says `tier: frontier | standard | fast`;
the harness maps it (`frontier` → Opus on Claude, a GPT-5-class model on Codex, Gemini Ultra
on Gemini, etc.). If an operator genuinely needs to pin a specific model, that is a *host-side
install override* (`5dive loop install ci-analyst --model=opus`), and it never enters the
shared file. Naming a SKU in `LOOP.md` would make the loop non-portable — the one thing the
format exists to prevent.

### 3.1 Skill resolution
The **recommended** form is an explicit `owner/repo/skill` path — unambiguous, and it mirrors
how skills.sh installs (`owner/repo`). A bare name is allowed as an npm-style convenience.

```yaml
skills:
  - 5dive-ai/skills/deep-research   # owner/repo/skill — explicit (recommended)
  - acme/scrapers/competitor-scrape # any public skills repo
  - deep-research                   # bare — convenience, resolved per below
```

Each entry resolves in three tiers, in order:

1. **Host-satisfied.** If the harness already provides a skill of that name (a built-in, or
   one already installed), it is used as-is and nothing is fetched. So `deep-research` on a
   5dive box uses our bundled skill; on Claude Code it uses a built-in if present.
2. **Path-resolved.** An `owner/repo/skill` path is fetched directly from that public repo —
   no registry, no ambiguity. This is the form the examples use.
3. **Registry-resolved (bare names).** A bare name resolves against a default skills registry
   (e.g. the skills.sh index, already a `name → owner/repo` map): the canonical/"official"
   entry is fetched. The npm model — you write `react`, not `github.com/facebook/react`.
   Authors **should** prefer the explicit path whenever a name might collide; on collision the
   resolver prefers the host's own skill, then the registry's official entry, and warns.

An unresolvable skill is skipped with a warning, not a hard failure — a loop degrades rather
than refusing to install.

### 3.2 Environment & pre-flight (`requires`)
A skill is knowledge; a loop *runs*, so it must declare the environment it needs. The
`requires` block lets the installer **pre-flight** a loop before it ever fires:

```yaml
requires:
  cli: [gh, ffmpeg]          # binaries that must be on PATH
  secrets: [X_API_TOKEN]     # env-var NAMES only
  mcp: [github]              # MCP servers the loop calls
  network: [api.x.com]       # egress the loop needs
```

**`requires` is a manifest of expectations, not an installer.** Only `skills` are ever
fetched. The `cli`/`secrets`/`mcp`/`network` entries declare what must *already* be true in
the environment; the runtime **checks** them and never auto-installs binaries, secrets, or
servers (doing so across every harness/OS would be a security and portability hole).

- **`cli` / `mcp` / `network` are declare-and-check.** On install the runtime pre-flights each
  one — is the binary on PATH, is the MCP server configured, is that egress allowed — and
  **prompts for what's missing or refuses to install**, rather than failing silently on the
  first scheduled run at 3am. It never silently launches an MCP server (those run arbitrary
  code — consent required). MCP servers do have registries (Smithery, mcp.so), so a capable
  host *may* offer to provision a missing one, but that is host-optional, never spec-mandated.
- **`secrets` are names, never values.** A loop declares *that* it needs `X_API_TOKEN`; the
  value is supplied host-side at install (the installer prompts, or reads the box's secret
  store) and never enters `LOOP.md` or the public repo.
- `requires` is also the **trust surface**: a reader sees exactly which binaries, secrets,
  servers, and network a loop touches *before* running it.

Install is a one-liner, keyed to a GitHub `owner/repo` shorthand:

```bash
# any harness — auto-detects the harness in the current dir/env
npx agenticloops install <owner/loop>

# target a specific harness explicitly
npx agenticloops install <owner/loop> --harness=claude-code

# native on a 5dive runtime
5dive loop install <slug> --onto=<agent>
```

**`--harness` is optional.** By default the installer auto-detects the harness present in the
current project/environment (a `.claude` dir, a `.cursor` dir, a 5dive box, etc.) and installs
there, the way `npx skills add` detects installed agents. Pass `--harness=<id>` to target a
specific one, or to disambiguate when several are present.

A loop needs **scheduling**, not just a model. A harness must be able to both run the agent and
honor the trigger — runtimes with a scheduler (5dive, GitHub Actions, a cron-driven CLI) can;
an IDE that only runs interactively cannot. The installer warns if you target a harness that
can't honor the loop's trigger.

---

## 4. Multi-agent loops — `agents:`

Most loops are one agent doing one job on a timer. Some jobs are a **pipeline**: a researcher
gathers, a writer drafts, a publisher ships. A loop expresses that as an ordered chain of
roles under `agents:` — still one file, one trigger, one schedule, **one scheduled run**.

A multi-agent loop is **not** "spawn N processes." It is one run executed as an ordered
sequence of role-prompts, where each role's structured output feeds the next. That is the
natural extension of §2's multi-heading body (several prompts run in order) — with a persona,
optional per-role skills, and an explicit `{{previous_output}}` handoff per step.

```yaml
---
name: intel-brief
description: Competitive-intel pipeline — a researcher gathers what changed, a writer turns it into a sourced briefing.
schedule: every 4h
tier: frontier
effort: high
agents:
  - role: researcher                       # required — kebab id, unique in the loop
    persona: dude                          # optional — cosmetic, as in §2
    skills: [deep-research, compile-knowledge]   # optional — per-role, additive to top-level skills
    prompt: |
      Scan our competitor set and the field for the last interval — launches,
      pricing, funding, notable chatter. Return a structured list of what
      changed, with sources. No prose, just the findings.
  - role: writer
    persona: theo
    skills: [copywriting]
    prompt: |
      From the researcher's findings below, write a concise sourced briefing of
      what changed and what it means for us, then post it to the team.
      Findings:
      {{previous_output}}
tags: [research, market-intel, multi-agent]
license: MIT
---
```

**Semantics.**

- Roles run in **array order**, strictly **sequential** — no branching or parallel fan-out in
  v0.1. The chain is the loop's single scheduled run.
- Each role receives the **prior role's output** as context, injected wherever the prompt
  writes `{{previous_output}}` (or prepended if the placeholder is absent). The first role
  gets none.
- `skills` may be declared top-level (shared by every role) and/or per-role (additive). A role
  with no `skills` inherits the loop's top-level set.
- `trigger` (`schedule`/`event`), `requires`, `tier`, `effort`, `concurrency`, `timeout`, and
  `tags` stay **top-level** — they govern the whole run, not one role.
- **Back-compat is total.** No `agents:` block → the single starter-prompt body *is* the loop,
  exactly as §1. A host that doesn't understand `agents:` ignores it (unknown fields never
  error, §5) and can still surface the loop in discovery.

**Handoff is structured, not chat.** Each role's output passes to the next as a defined
artifact (a structured result, injected at `{{previous_output}}`) — never best-effort
transcript scraping. That is the one hard requirement, and it's what makes an unattended run
deterministic anywhere.

**Execution is the host's business — and that's the point.** The spec mandates *what* (ordered
roles, structured handoff), never *how* a host runs them. The simplest conforming
implementation is a **for-loop over prompts**: run role 1, capture its output, run role 2 with
that output substituted — no process spawning at all, which is why any harness with a scheduler
can do it (a reference runner does exactly this in ~40 lines, shelling to whatever model CLI is
present). A host with real multi-agent orchestration *may* instead give each role its own
isolated agent context (cleaner, and parallel-able in a later spec) — e.g. on a 5dive runtime
each role runs as a linked task that hands off a structured result. **Both satisfy the same
observable contract: role *N* sees role *N-1*'s structured output.** The `LOOP.md` is
byte-identical across them, so no execution model leaks into the portable file.

---

## 5. Discovery (auto-indexing)

The directory is **not** hand-curated. Any public repo that (a) carries the GitHub topic
`agenticloops` and (b) contains a valid `LOOP.md` is discoverable. A crawler walks the
GitHub search API on a schedule, validates each match against this spec, and rebuilds the
index. Publishing a loop = pushing a conforming repo and tagging it. That's it.

The ranking metric is **proof, not popularity**: a loop that emits verifiable, co-signed
run receipts outranks one that just has stars. (More in the registry docs.)

---

## 6. Versioning

This spec is `v0.1`. Breaking changes bump the minor version until `1.0`. A loop may pin
`spec: 0.1` in frontmatter; absence means "latest". Unknown fields are ignored, not errors,
so the format can grow without breaking older hosts.

---

_Maintained at [github.com/5dive-ai/loops](https://github.com/5dive-ai/loops). MIT._
