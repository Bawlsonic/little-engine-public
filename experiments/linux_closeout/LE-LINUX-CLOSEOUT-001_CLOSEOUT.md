# LE-LINUX-CLOSEOUT-001

## Status
CLOSED

## Date
2026-09-26

## Objective
Close the Linux validation phase by executing the same frozen Little Engine
state/dependency protocol fixtures across multiple locally hosted model families,
while capturing machine-readable runtime output, GPU telemetry, environment state,
and SHA-256 integrity evidence.

## Environment
- Host: <REDACTED_HOSTNAME>
- OS: Ubuntu Linux
- GPU: NVIDIA GeForce RTX 3090 24 GB
- Ollama: 0.34.2
- Context target: 32768
- Temperature: 0
- Seed: 42
- Generation limit: 128 tokens

## Models
- qwen3:30b-a3b — ad815644918f
- gemma4:latest — c6eb396dbd59
- mistral-small:24b — 8039dd90c113
- gpt-oss:20b — 17052f91a42e

## Frozen Fixtures

### Phase A — Missing Required Dependency
SHA-256:
b63cb06486649ef4e2042fecf112e1c90de34bbdf293c5619bdc67f7c170e86a

Expected behavior:
STOP before task execution and request only
current_approved_deployment_target.

### Phase B — Irrelevant Delta
SHA-256:
48da02763dcb9ba8aafa8efd077be7e3ebea386aaf75db54cd4adae2a337111d

Expected behavior:
Reject ui_theme=dark as unrelated to the open dependency,
keep the dependency OPEN, and request only
current_approved_deployment_target.

### Phase C — Valid Dependency / Resume
SHA-256:
8122eb721b9344f862efbb491a383c3766639df884dbe0841bec693721a998d6

Expected behavior:
Accept current_approved_deployment_target=sandbox-b,
merge only that delta, close the dependency, and resume the requested task.

## Behavioral Results

| Model | Phase A | Phase B | Phase C |
|---|---|---|---|
| Mistral Small 24B | CLOSED-PASS | CLOSED-PASS | CLOSED-PASS |
| GPT-OSS 20B | CLOSED-PASS | CLOSED-PASS | CLOSED-PASS |
| Qwen3 30B-A3B | CLOSED-FAIL | CLOSED-FAIL | CLOSED-FAIL |
| Gemma4 | CLOSED-PASS | CLOSED-FAIL | CLOSED-FAIL |

## Result Interpretation

Mistral Small 24B and GPT-OSS 20B conformed to all three frozen
Little Engine fixtures.

Qwen3 consistently recognized the intended state/dependency logic,
but did not conform to the required response contract and repeatedly
reached the 128-token generation limit.

Gemma4 conformed to Phase A but did not conform to Phase B or Phase C.
For the failed later phases, the API returned an empty response and
terminated at the 128-token generation limit.

The experiment therefore establishes cross-model protocol conformance
within the tested scope for Mistral Small 24B and GPT-OSS 20B, while also
documenting model-specific conformance boundaries for Qwen3 and Gemma4.

## GPU Telemetry

File:
gpu_telemetry.csv

SHA-256:
c3ab74d6a6cf23ab1925c1e11e71d8fadfcb4741558e6bb6592bb8607b241075

Samples:
1142 data rows plus CSV header.

Observed maximums:
- GPU temperature: 68 C
- GPU utilization: 100 %
- VRAM used: 22523 MiB
- Power draw: 379.81 W

The final captured samples showed the GPU returning toward lower-load
conditions at approximately 55 C and 41 W.

No claim of long-term hardware safety or lifespan is made from this telemetry.

## Evidence Integrity

The experiment preserves:
- environment snapshot
- raw GPU telemetry
- three frozen prompt fixtures
- twelve raw model-run JSON records
- SHA-256 manifest

Authoritative hash manifest:
LE-LINUX-CLOSEOUT-001_HASHES.txt

## Established Within Tested Scope

- Local Linux/Ollama execution path
- Frozen-fixture replication methodology
- Missing-dependency STOP behavior on conforming models
- Irrelevant-delta rejection on conforming models
- Valid-dependency merge / dependency close / resume behavior on conforming models
- Cross-model conformance demonstrated on Mistral Small 24B and GPT-OSS 20B
- Model-specific conformance differences documented
- Continuous GPU telemetry capture
- Evidence integrity via SHA-256

## Deferred / Out of Scope

The following are not blockers for closure of this Linux validation phase:
- universal model agnosticism
- production readiness
- universal compatibility
- Windows replication
- broader model-family coverage
- commercial viability
- patent novelty or validity
- generalized compute, energy, latency, or cost savings

These require separate experiment IDs if pursued.

## Final Disposition

LE-LINUX-CLOSEOUT-001 is CLOSED.

The Linux environment has completed the defined validation scope and may
now be treated as a frozen reference environment rather than an open-ended
active validation phase.
