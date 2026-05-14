# Agent Context — openclaw-hardened

> Machine-readable project metadata for AI agents. Humans: see [README.md](README.md).

## Purpose

Security-first deployment blueprint for running OpenClaw on a self-hosted VPS. Covers 4-layer permission model, systemd hardening, supply chain lockdown, memory tuning, skill architecture, cost optimization, and production operations. The guide explains the reasoning behind every decision, not just the steps.

## Start Here

1. `CLAUDE.md` — Architecture, conventions, gotchas, Beads doctrine (read FIRST). [→ CLAUDE.md](CLAUDE.md)
2. `AGENTS.md` — This file: machine-readable project context + Beads commands.
3. `README.md` — Project overview, audience, navigation. [→ README.md](README.md)
4. `GUIDE.md` — The full deployment guide (15 phases, 2,800+ lines). [→ GUIDE.md](GUIDE.md)
5. Reference docs — Deep dives on specific topics (see File Index below).

## Beads Commands (lifecycle)

This repo uses Beads (`bd`) for task tracking. Auto-memory at `~/.claude/projects/-home-mj-projects-openclaw-bot/memory/` is for narrative continuity, NOT a substitute.

```bash
bd ready --json                                    # find available work
bd show <id> --json                                # view issue details
bd update <id> --claim --json                      # claim work
bd comment <id> "progress observation" --json      # concurrent observations during work
bd note <id> "durable summary" --json              # consolidated state visible in `bd show`
bd close <id> --reason "completed" --json          # close on real completion / merge / supersession
bd dolt pull || true                               # at session start (if remote configured)
bd dolt push                                       # at session end
bd batch <file>                                    # scripted multi-bead mutations
```

See **CLAUDE.md → Beads Conventions (Repo Override)** for the creation-time contract on every `bd create` (`--description`, `--context`, `--notes` with SOURCES/kn entry).

## File Index

| File | Format | Lines | Purpose | Relevance |
|------|--------|-------|---------|-----------|
| `README.md` | Markdown | ~60 | Project overview, quick start, installer | [essential] |
| `install.sh` | Bash | ~180 | Safe-defaults installer for VPS tooling | [essential] |
| `GUIDE.md` | Markdown | 2,700 | Full deployment guide — 14 phases + 8 appendices | [essential] |
| `AGENTS.md` | Markdown | ~80 | This file — machine-readable project context | [essential] |
| `Reference/SECURITY.md` | Markdown | 2,600 | VPS/OS hardening, application/LLM security (55 sources) | [reference] |
| `Reference/COST-AND-ROUTING.md` | Markdown | 580 | LLM cost optimization, model routing, provider strategy | [reference] |
| `Reference/IDENTITY-AND-BEHAVIOR.md` | Markdown | 590 | System prompt design, persona patterns, identity security | [reference] |
| `Reference/SKILLS-AND-TOOLS.md` | Markdown | 570 | Skill architecture, tool permissions, supply chain security | [reference] |
| `Reference/MEMORY-PLUGIN-RESEARCH.md` | Markdown | 400 | mem0 evaluation, built-in memory optimization | [reference] |
| `Reference/CONTEXT-ENGINEERING.md` | Markdown | 245 | Prompt caching, session persistence, memory tuning | [reference] |
| `Reference/SECURITY-PATCHES.md` | Markdown | 107 | Version-specific security patches and action status | [reference] |
| `Reference/UPGRADE-NOTES.md` | Markdown | 480 | Changelog across OpenClaw releases with deployment impact | [reference] |
| `Reference/DATABASE-MAINTENANCE.md` | Markdown | ~120 | Compaction loop prevention, bot database baseline | [reference] |
| `Reference/VOICE-AND-AUDIO.md` | Markdown | ~350 | STT research: cloud/self-hosted providers, Telegram voice, architecture patterns | [reference] |
| `Reference/KNOWN-BUGS.md` | Markdown | ~250 | Systemic bugs: duplicate messages (7 root causes), silent polling death, cost impact | [reference] |
| `Reference/GREGOR-CRABBOX-RUNBOOK.md` | Markdown | ~250 | Telegram-to-Gregor Crabbox execution workflow for project coding tasks | [reference] |
| `Reference/CRABBOX-HARDENING.md` | Markdown | ~360 | Crabbox doctrine: 2-component architecture (CLI+plugin), plugin-vs-shell fork, E2B-first provider verdict, 15 hardening rules, lease lifecycle, sync/secrets discipline | [reference] |
| `.crabboxignore` | Config | 25 | Crabbox sync exclusions: Interceptor screenshots, Plans/, .sessions/, local audit artifacts | [config] |
| `Reference/DEFENSE-SYSTEM.md` | Markdown | ~400 | 6-layer prompt injection defense: architecture, STRIDE review, deployment, trajectory bundles | [reference] |
| `Reference/THREAT-MODEL-CROSSREF.md` | Markdown | ~250 | Berman 6-layer ↔ MITRE ATLAS 16-threat mapping; gap analysis | [reference] |
| `Reference/MORITZ-BLUEPRINT.md` | Markdown | ~280 | Moritz Kram OpenClaw masterclass (fd4k16REDOU) mapped to Gregor: 10-step gap analysis, 5 bead actions, publish-back angles | [reference] |
| `src/defense/` | TypeScript | ~1800 | 6-layer defense system: sanitizer, scanner, gate, redaction, governor, access control | [security] |
| `src/defense/plugin/` | TypeScript | ~570 | Defense plugin: 5 hook events covering all 6 layers (primary enforcement) | [security] |
| `src/defense/proxy/` | TypeScript | ~300 | Defense proxy: Bun HTTP server (inactive, code preserved) | [security] |
| `src/defense/__tests__/` | TypeScript | ~1200 | 162 tests covering all defense layers and attack vectors | [tests] |
| `Reference/CLAWKEEPER.md` | Markdown | ~300 | ClawKeeper adoption: installation, audit domains, commands, comparison | [reference] |
| `src/config/openclaw.json.example` | JSON | 93 | Sanitized config template with security annotations | [config] |
| `src/config/logrotate-openclaw` | Config | 15 | Log rotation configuration | [utility] |
| `src/scripts/backup.sh` | Bash | 49 | Daily backup with 30-day retention | [utility] |
| `src/scripts/health-check.sh` | Bash | 83 | Service health monitoring | [utility] |
| `src/scripts/verify-binding.sh` | Bash | 38 | Gateway binding verification | [utility] |
| `src/scripts/auto-update.sh` | Bash | 66 | Weekly update + security audit | [utility] |
| `src/pipeline/send.sh` | Bash | 40 | Pipeline: send message to bot — RETIRED 2026-04-28 | [archival] |
| `src/pipeline/read.sh` | Bash | 43 | Pipeline: read bot responses — RETIRED 2026-04-28 | [archival] |
| `src/pipeline/status.sh` | Bash | 30 | Pipeline: check pipeline status (now fail-fasts on missing VPS dir) | [archival] |
| `src/pai-pipeline/pai-submit.sh` | Bash | 109 | PAI pipeline: submit task to local agent | [utility] |
| `src/pai-pipeline/pai-result.sh` | Bash | 179 | PAI pipeline: read results with wait/ack modes | [utility] |
| `src/pai-pipeline/pai-status.sh` | Bash | 106 | PAI pipeline: dashboard (human + JSON output) | [utility] |
| `src/pai-pipeline/pai-result-notify.sh` | Bash | ~160 | PAI pipeline: result notification to bot's inbox | [utility] |
| `src/pai-pipeline/pai-result-watcher.py` | Python | ~100 | PAI pipeline: inotify watcher for results/ | [utility] |
| `src/pai-pipeline/pai-notify.service` | systemd | 20 | PAI pipeline: watcher service unit | [config] |
| `src/pai-pipeline/pai-escalation-submit.sh` | Bash | ~80 | PAI pipeline: auto-escalation handler (Layer 5) | [utility] |
| `src/pai-pipeline/pai-escalation.path` | systemd | 15 | PAI pipeline: watches escalate/ for new files | [config] |
| `src/pai-pipeline/pai-escalation.service` | systemd | 12 | PAI pipeline: triggers escalation submit script | [config] |
| `src/pai-pipeline/pai-reverse-handler.sh` | Bash | ~170 | PAI pipeline: reverse-task processor via `openclaw agent` (Layer 6) | [utility] |
| `src/pai-pipeline/pai-reverse-watcher.py` | Python | ~90 | PAI pipeline: inotify watcher for reverse-tasks/ | [utility] |
| `src/pai-pipeline/pai-reverse.service` | systemd | 20 | PAI pipeline: reverse-task watcher service | [config] |
| `src/pai-pipeline/pai-overnight.sh` | Bash | ~340 | PAI pipeline: overnight PRD queue coordinator (Layer 7) | [utility] |
| `src/pai-pipeline/pai-overnight-local.sh` | Bash | ~100 | PAI pipeline: local helper for overnight queue | [utility] |
| `assets/social-preview.png` | PNG | — | GitHub social preview image (1280x640) | [asset] |
| `Reference/CONCEPTS-INVENTORY.md` | Markdown | ~80 | OpenClaw concepts surveyed with adopt/defer/skip verdicts (memory engines, dreaming, active memory, experimental flags) | [reference] |

## Repository Map

```
README.md                             # Project overview + quick start
GUIDE.md                              # The deployment guide (2,800+ lines, 15 phases)
AGENTS.md                             # This file — machine-readable project context
install.sh                            # Safe-defaults installer for VPS tooling

Reference/
  SECURITY.md                         # VPS/OS hardening + application/LLM security (2,600 lines, 55 sources)
  COST-AND-ROUTING.md                 # Provider pricing, model routing, ClawRouter analysis
  IDENTITY-AND-BEHAVIOR.md            # System prompt design, persona patterns, identity-layer security
  SKILLS-AND-TOOLS.md                 # Skill architecture, tool permissions, supply chain security
  MEMORY-PLUGIN-RESEARCH.md           # Why built-in memory over external plugins (mem0 eval)
  CONTEXT-ENGINEERING.md              # Prompt caching, session persistence, memory tuning
  SECURITY-PATCHES.md                 # Version-specific security patches and action status
  UPGRADE-NOTES.md                    # Comprehensive changelog across OpenClaw releases
  DATABASE-MAINTENANCE.md             # Compaction loop prevention, bot database baseline
  VOICE-AND-AUDIO.md                  # STT research: cloud/self-hosted providers, Telegram voice, architecture
  KNOWN-BUGS.md                       # Systemic bugs: duplicate messages (7 root causes), silent polling death
  GREGOR-CRABBOX-RUNBOOK.md           # Telegram -> Gregor -> Crabbox coding execution workflow
  CRABBOX-HARDENING.md                # Crabbox doctrine: architecture, plugin-vs-shell fork, E2B-first, 15 hardening rules

src/
  config/
    openclaw.json.example             # Sanitized config template with security annotations
    logrotate-openclaw                # Log rotation config
  scripts/
    backup.sh                         # Daily backup with 30-day retention
    health-check.sh                   # Service health monitor
    verify-binding.sh                 # Gateway binding verification
    auto-update.sh                    # Weekly update + security audit
  pipeline/
    send.sh / read.sh / status.sh     # Async messaging pipeline (local → bot)
  pai-pipeline/                       # ARCHIVAL — Isidore Cloud retired 2026-04-27, source preserved for reference
  defense/
    layer1-sanitizer.ts               # L1: Deterministic text sanitizer
    layer2-scanner.ts                 # L2: LLM frontier scanner
    layer3-outbound.ts                # L3: Outbound content gate
    layer4-redaction.ts               # L4: Redaction pipeline
    layer5-governor.ts                # L5: Call governor (spend/volume/dedup/circuit breaker)
    layer6-access.ts                  # L6: Access control (path guards, URL safety, DNS pinning)
    patterns.ts                       # Shared secret patterns (18 definitions)
    types.ts / index.ts               # Types and entry point
    plugin/
      index.ts                        # Plugin registration (5 hook events, all 6 layers)
      hooks.ts                        # Hook handler factories
      types.ts                        # Plugin-specific types
      package.json                    # Plugin package manifest
    proxy/                            # Defense proxy (inactive, code preserved in repo)
    __tests__/                        # 162 tests across 6 files
  audit/
    audit.sh                          # Security audit prompts and tooling
```

## Architecture

### Agent Topology (post-2026-04-27)

A single always-on bot runs on the VPS as the primary agent. The dual-agent / shared-filesystem-pipeline architecture documented in earlier revisions was retired 2026-04-27.

> Names used here (primary bot) are generic -- substitute your actual bot name.

- **Primary bot** (`openclaw` user) — OpenClaw/Sonnet via Anthropic. Always-on Telegram bot. Compaction routes to OpenRouter (`openrouter/openai/gpt-4.1-mini`); embeddings via local Ollama (`nomic-embed-text:v1.5`).
- **Secondary bot** (`dismas` user) — OpenClaw/GPT-5.4 via OpenAI Codex OAuth. Currently DOWN; redeploy deferred until a concrete capability gap is named.
- **Retired 2026-04-27:** Isidore Cloud (`isidore_cloud` user, Claude Code/Opus on-demand heavy compute) and the bidirectional PAI pipeline at `/var/lib/pai-pipeline/`. `src/pai-pipeline/` source is retained in the repo as archival history. `Reference/PAI-PIPELINE.md` was deleted at retirement; consult git history for the as-built architecture (`git log -- Reference/PAI-PIPELINE.md`).
- **Under evaluation:** Gregor → Codex CLI delegation for coding-only tasks (Phase 1: per-turn `model` override on `message_received` → `gpt-5.3-codex-spark`; Phase 2: bundled `codex-harness` plugin v2026.4.10+).

### 6-Layer Prompt Injection Defense

A native OpenClaw plugin enforces the 6-layer defense system via 5 gateway hooks. Built with Bun + TypeScript. Based on Matthew Berman's 6-layer architecture, informed by Pliny the Prompter's attack research.

- **L1: Deterministic Sanitizer** (`message_received` hook) — Unicode NFKC + homoglyph map, base64/base64url/hex/ROT13 decoding, HTML/markdown stripping, system prompt override detection (16 patterns), role injection (8 patterns), zero-width/Zalgo/PUA/emoji stego/whitespace stego removal, wallet address flagging. 100KB limit for ReDoS prevention.
- **L2: LLM Frontier Scanner** (`message_received` hook, high-risk channels only) — Nonce-delimited classification prompt, risk scoring 0-100. Fires conditionally: skips Telegram DMs (trusted), triggers on untrusted channels when L1 detects ambiguous input. Auto-activates via plugin runtime when Anthropic auth is available.
- **L3: Outbound Content Gate** (`message_sending` + `llm_output` hooks) — Leaked secrets (18 patterns), internal paths (Unix+Windows+UNC), injection artifacts, exfil URLs, financial data. Modifies outbound messages to redact violations before delivery.
- **L4: Redaction Pipeline** (`message_sending` + `llm_output` hooks) — API keys/tokens, personal emails (50+ provider domains, work subdomains preserved), phone numbers, dollar amounts. Pre-delivery enforcement.
- **L5: Call Governor** (`llm_input` hook) — Rolling-window spend limits (monotonic clock), volume limits with per-caller overrides, lifetime counter, caller-scoped SHA-256 dedup, circuit breaker. Tracking only (void hook).
- **L6: Access Control** (`before_tool_call` hook) — Path guards (30+ denied filenames, 18 denied extensions), URL safety (IPv4+IPv6 private ranges, DNS resolution with 3s timeout). Blocks tool calls to sensitive paths/URLs.

**Plugin:** 5 hook events registered via `api.registerHook()`, covering all 6 layers (L1+L2 share `message_received`).

**Tests:** 162 tests across 6 files. STRIDE threat model + security review applied (3 CRITICALs, 7 HIGHs, 6 MEDIUMs, 4 LOWs all fixed).

### ClawKeeper (Security Monitor)

ClawKeeper v0.1.0 — security auditing and monitoring plugin. Complements Defense Shield with passive observation:

- **Config audit:** 9 security domains, scores 0-100, actionable findings with auto-fix support
- **Drift detection:** Watches `openclaw.json` and `AGENTS.md` for boundary changes
- **Skill scanning:** Static analysis for dangerous patterns (`scan-skill` command)
- **Event logging:** All 5 hook events to JSONL at `~/.openclaw/workspace/log/`
- **Commands:** `openclaw clawkeeper audit`, `clawkeeper logs --scan`, `clawkeeper scan-skill`

See [Reference/CLAWKEEPER.md](Reference/CLAWKEEPER.md) for full reference.

### Architecture Decisions

Key choices that an agent should understand before suggesting modifications:

- **Security model:** 4-layer permission pipeline — `tools.profile` (coarse), `tools.alsoAllow/deny` (fine), `exec.security` (shell), `ask` mode (runtime). Current bot config: `profile: "full"`, `alsoAllow: ["cron", "browser"]`, `deny: ["gateway", "nodes"]`, `exec.security: "full"`. Documented in GUIDE.md Phase 7.
- **LLM provider setup:** The guide covers provider configuration (Anthropic, OpenRouter, Ollama) with cost analysis and routing strategy.
- **Bundled-only skills strategy:** Zero community (ClawHub) skill installs. Only the 50 bundled skills are used. Rationale: supply chain attack surface (see Reference/SKILLS-AND-TOOLS.md for the ClawHavoc case study).
- **Local embeddings:** Uses `embeddinggemma-300m` locally instead of cloud-based OpenAI embeddings. Deliberate privacy + cost decision.
- **Loopback-only gateway:** Gateway bound to 127.0.0.1:18789, never exposed to the internet. All external access via Telegram integration.
- **No privilege escalation in cross-agent pipeline:** Agents communicate via group-writable files, never sudo/su.

## Conventions

- **Commit messages:** Clear "why" with area prefix when helpful (e.g., `docs: add skills research`)
- **File naming:** kebab-case
- **Documentation:** Thorough — the guide includes reasoning blocks, not just commands
- **What to read before modifying:** This file, then README.md, then the specific section of GUIDE.md relevant to your change

<!-- BEGIN BEADS INTEGRATION v:1 profile:minimal hash:ca08a54f -->
## Beads Issue Tracker

This project uses **bd (beads)** for issue tracking. Run `bd prime` to see full workflow context and commands.

### Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --claim  # Claim work
bd close <id>         # Complete work
```

### Rules

- Use `bd` for ALL task tracking — do NOT use TodoWrite, TaskCreate, or markdown TODO lists
- Run `bd prime` for detailed command reference and session close protocol
- Use `bd remember` for persistent knowledge — do NOT use MEMORY.md files

## Session Completion

**When ending a work session**, you MUST complete ALL steps below. Work is NOT complete until `git push` succeeds.

**MANDATORY WORKFLOW:**

1. **File issues for remaining work** - Create issues for anything that needs follow-up
2. **Run quality gates** (if code changed) - Tests, linters, builds
3. **Update issue status** - Close finished work, update in-progress items
4. **PUSH TO REMOTE** - This is MANDATORY:
   ```bash
   git pull --rebase
   bd dolt push
   git push
   git status  # MUST show "up to date with origin"
   ```
5. **Clean up** - Clear stashes, prune remote branches
6. **Verify** - All changes committed AND pushed
7. **Hand off** - Provide context for next session

**CRITICAL RULES:**
- Work is NOT complete until `git push` succeeds
- NEVER stop before pushing - that leaves work stranded locally
- NEVER say "ready to push when you are" - YOU must push
- If push fails, resolve and retry until it succeeds
<!-- END BEADS INTEGRATION -->
