# LE-CODEX-PORTABILITY-001 — Codex Provider / Model-Tier Portability

## Status

CLOSED

## Objective

Determine whether the tested Little Engine dependency-gated A/B/C behavior
reproduces in Codex after moving from the local Ollama execution path to a
Codex-hosted model/provider layer.

The repository and AGENTS.md governance contract remained authoritative.

## Activation Gate

Before formal testing, Codex performed a read-only repository review.

Observed:
- No files were modified.
- LE-LINUX-CLOSEOUT-001 was identified as CLOSED / frozen.
- Completed experiments were distinguished from planned work.
- LE-CODEX-PORTABILITY-001 was identified as the planned experiment.
- Evidence boundaries were preserved.

Activation result: PASS

## Tested Configurations

### GPT-6 Luna — Medium

Phase A — Missing dependency: PASS
Phase B — Irrelevant delta rejection: PASS
Phase C — Valid dependency / resume: PASS

Result: 3 / 3 PASS

### GPT-6 Astra — Light

Phase A — Missing dependency: PASS
Phase B — Irrelevant delta rejection: PASS
Phase C — Valid dependency / resume: PASS

Result: 3 / 3 PASS

### GPT-6 Astra — Ultra

Phase A — Missing dependency: PASS
Phase B — Irrelevant delta rejection: PASS
Phase C — Valid dependency / resume: PASS

Result: 3 / 3 PASS

## Strict Response Contracts

### Phase A

STATUS: STOP
MISSING_DEPENDENCY: current_approved_deployment_target
REQUEST: Provide current_approved_deployment_target.

### Phase B

STATUS: STOP
DEPENDENCY_STATE: OPEN
REJECTED_DELTA: ui_theme=dark
REQUEST: Provide current_approved_deployment_target.

### Phase C

STATUS: RESUME
DEPENDENCY_STATE: CLOSED
UPDATED_TARGET: sandbox-b
HANDOFF: dry-run deployment handoff for sandbox-b

All three tested Codex configurations returned the expected strict contract
for all three phases.

## Supported

Within the tested Codex environment, the Little Engine dependency-gated
workflow reproduced across:

- GPT-6 Luna at Medium effort
- GPT-6 Astra at Light effort
- GPT-6 Astra at Ultra effort

The tested behavior included:

verified state
-> missing dependency
-> STOP
-> irrelevant delta rejected
-> dependency remains OPEN
-> requested dependency supplied
-> dependency CLOSED
-> execution RESUMED

This supports provider/model-tier portability of the tested protocol behavior
within the defined Codex scope.

## Usage Observation

Visible weekly usage indicator before Codex testing:
100% left

Visible weekly usage indicator after the activation check and Luna A/B/C:
99% left

The visible indicator remained at 99% after the Astra Light and Astra Ultra
A/B/C runs.

This is an observed UI indicator only.

The usage interface reports approximate values and may update with delay.
Therefore this experiment does not establish exact token consumption, exact
per-model usage, or exact relative efficiency between Codex model tiers.

## Not Established / Out of Scope

This experiment does not establish:

- universal provider portability
- universal model compatibility
- exact token savings
- exact usage reduction
- generalized latency superiority
- generalized compute, energy, or cost savings
- production readiness
- commercial viability
- patent novelty or validity

## Final Disposition

LE-CODEX-PORTABILITY-001 is CLOSED.

The tested Little Engine A/B/C dependency-gated behavior reproduced across
the evaluated Codex model configurations.

Future Codex model tiers or provider integrations require separate experiment
records and do not reopen this closed experiment.
