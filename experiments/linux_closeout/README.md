# LE-LINUX-CLOSEOUT-001 — Linux Multi-Model Validation Closeout

## Validation Phase Closed

This experiment closes the defined Linux validation phase for Little Engine on the local NVIDIA RTX 3090 / Ollama environment.

The same frozen dependency-gated fixtures were executed across four locally hosted model families while preserving raw model responses, runtime metrics, continuous GPU telemetry, environment state, and SHA-256 integrity evidence.

## Evidence Boundary

### Observed

- The test host was `<REDACTED_HOSTNAME>` running Ubuntu Linux.
- The runtime was Ollama 0.34.2.
- The GPU was an NVIDIA GeForce RTX 3090 with 24 GB VRAM.
- Four locally hosted models were included:
  - `qwen3:30b-a3b`
  - `gemma4:latest`
  - `mistral-small:24b`
  - `gpt-oss:20b`
- Three frozen fixtures were used:
  - Phase A — missing required dependency
  - Phase B — irrelevant incoming delta
  - Phase C — valid dependency / resume
- The fixture files were SHA-256 hashed before evaluation.
- Each model received the same phase-specific fixture.
- Raw Ollama JSON responses were preserved for every model/phase combination.
- Continuous NVIDIA GPU telemetry was captured during the closeout session.
- The telemetry file contains 1,142 data rows plus the CSV header.
- Maximum observed GPU utilization was 100%.
- Maximum observed GPU temperature was 68 C.
- Maximum observed VRAM usage was 22,523 MiB.
- Maximum observed power draw was 379.81 W.
- Final telemetry samples showed the GPU returning toward lower-load conditions at approximately 55 C and 41 W.
- Mistral Small 24B passed all three strict-conformance fixtures.
- GPT-OSS 20B passed all three strict-conformance fixtures.
- Qwen3 30B-A3B did not satisfy the strict response contract in any of the three fixtures and repeatedly terminated at the configured 128-token generation limit.
- Gemma4 passed Phase A and did not satisfy the strict response contract in Phase B or Phase C.
- The complete evidence set was SHA-256 hashed.
- A final closeout record was created and hashed.

## Behavioral Matrix

| Model | Phase A — STOP | Phase B — Reject | Phase C — Resume |
|---|---|---|---|
| Mistral Small 24B | PASS | PASS | PASS |
| GPT-OSS 20B | PASS | PASS | PASS |
| Qwen3 30B-A3B | CLOSED-FAIL | CLOSED-FAIL | CLOSED-FAIL |
| Gemma4 | PASS | CLOSED-FAIL | CLOSED-FAIL |

A CLOSED-FAIL result represents a completed experiment that did not satisfy the defined strict-conformance criterion. It does not leave the validation phase open.

### Supported

This experiment supports that the frozen Little Engine dependency-gated workflow can be executed consistently across multiple locally hosted model families within the tested Linux/Ollama environment.

Within the tested scope, Mistral Small 24B and GPT-OSS 20B demonstrated the complete progression:

verified state
→ missing dependency
→ STOP
→ unrelated delta
→ dependency remains OPEN
→ requested dependency supplied
→ validated delta merged
→ dependency CLOSED
→ execution resumed

The result also supports that conformance behavior differs by model implementation. The same fixtures and test environment produced strict conformance on Mistral Small 24B and GPT-OSS 20B while exposing repeatable conformance boundaries on Qwen3 and Gemma4.

The captured evidence further supports that the dependency gate can operate as a compact state-control mechanism without requiring an open-ended interactive session when the evaluated model follows the response contract.

## Deferred / Out of Scope

The following are separate future workstreams and are not blockers for closure of this Linux validation phase:

- universal model agnosticism
- behavior across all model families
- behavior across all operating systems
- Windows replication
- production deployment readiness
- generalized compute savings
- generalized energy savings
- generalized latency improvement
- generalized cost reduction
- universal compatibility
- commercial viability
- patentability or novelty

These require separate experiment IDs if pursued.

## Evidence Artifacts

The experiment directory contains:

- `environment_snapshot.txt`
- `gpu_telemetry.csv`
- `phase_a_prompt.txt`
- `phase_b_irrelevant_prompt.txt`
- `phase_c_valid_dependency_prompt.txt`
- twelve raw model-run JSON records
- `LE-LINUX-CLOSEOUT-001_HASHES.txt`
- `LE-LINUX-CLOSEOUT-001_CLOSEOUT.md`

## Experiment Status

**LE-LINUX-CLOSEOUT-001: CLOSED**

The defined Linux validation scope is complete.

The Linux system may now be treated as a frozen reference environment. Any Windows migration, broader model testing, orchestration integration, or additional portability work should be performed under new experiment IDs without modifying this closed evidence set.
