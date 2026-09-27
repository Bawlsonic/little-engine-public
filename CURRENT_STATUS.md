# Current Validation Status

**Little Engine is in controlled experimental validation.**

## Closed Evidence

- **LE-LINUX-CLOSEOUT-001 — CLOSED**
  - Linux / RTX 3090 / Ollama multi-model validation complete.
  - Mistral Small 24B: 3 / 3 PASS.
  - GPT-OSS 20B: 3 / 3 PASS.
  - Qwen3 and Gemma4 conformance boundaries preserved as completed results.

- **LE-WINDOWS-PORTABILITY-001 — CLOSED**
  - Same RTX 3090.
  - Same Mistral model artifact.
  - Same Ollama runtime family.
  - Byte-identical A/B/C fixtures.
  - Windows replication: 3 / 3 PASS.

- **LE-CODEX-PORTABILITY-001 — CLOSED**
  - Codex governance activation: PASS.
  - GPT-6 Luna / Medium: 3 / 3 PASS.
  - GPT-6 Astra / Light: 3 / 3 PASS.
  - GPT-6 Astra / Ultra: 3 / 3 PASS.
  - Visible weekly usage moved from 100% left to 99% left during the overall Codex test window.
- **LE-CLAUDE-CODE-PORTABILITY-001 - CLOSED**
  - Claude Code desktop / Opus 5 / High.
  - Activation gate: PASS.
  - Phase A: PASS.
  - Phase B: PASS.
  - Phase C: PASS.
  - Run 01: 3 / 3 recorded PASS.
  - Frozen Windows A/B/C fixtures used as authoritative source.
  - Per-turn token and latency telemetry: NOT CAPTURED.
  - Bundled desktop runtime version: NOT CAPTURED.
  - Behavioral portability supported only within the tested Claude Code configuration.

## Supported Within Tested Scope

Little Engine's dependency-gated workflow has reproduced across:

- Linux and Windows
- local Ollama execution
- multiple local model families
- Codex-hosted model tiers
- Claude Code desktop / Opus 5 tested configuration

Tested sequence:

VERIFIED STATE
-> missing dependency
-> STOP
-> irrelevant delta rejected
-> dependency remains OPEN
-> requested dependency supplied
-> dependency CLOSED
-> execution RESUMED

## Evidence Boundary

This project does not currently claim:

- universal model agnosticism
- universal provider or operating-system portability
- production readiness
- exact token savings across providers
- generalized compute, energy, latency, or cost savings
- commercial viability
- patent novelty or validity

Closed experiments are frozen. New claims require new experiment IDs and evidence.
