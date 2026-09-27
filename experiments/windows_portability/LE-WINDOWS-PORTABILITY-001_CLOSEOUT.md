# LE-WINDOWS-PORTABILITY-001

## Status
CLOSED

## Date
2026-09-26

## Objective
Determine whether the previously validated Little Engine dependency-gated
workflow reproduces from Linux to Windows while holding the primary hardware,
model artifact, runtime family, test fixtures, and generation settings constant.

## Controlled Comparison

Linux reference:
- NVIDIA GeForce RTX 3090 24 GB
- Ollama
- mistral-small:24b
- Model ID: 8039dd90c113
- Context: 32768
- Temperature: 0
- Seed: 42
- Generation limit: 128 tokens

Windows replication:
- Windows 11 Pro
- NVIDIA GeForce RTX 3090 24 GB
- Ollama 0.34.4
- mistral-small:24b
- Model ID: 8039dd90c113
- Context: 32768
- Temperature: 0
- Seed: 42
- Generation limit: 128 tokens

Primary changed variable:
- Operating system: Linux -> Windows

## Frozen Fixture Equivalence

### Phase A — Missing Required Dependency
SHA-256:
b63cb06486649ef4e2042fecf112e1c90de34bbdf293c5619bdc67f7c170e86a

Result:
CLOSED-PASS

Observed response:
- STATUS: STOP
- requested only current_approved_deployment_target
- done_reason: stop
- prompt_eval_count: 365
- eval_count: 32

### Phase B — Irrelevant Delta
SHA-256:
48da02763dcb9ba8aafa8efd077be7e3ebea386aaf75db54cd4adae2a337111d

Result:
CLOSED-PASS

Observed response:
- STATUS: STOP
- dependency remained OPEN
- ui_theme=dark rejected
- requested only current_approved_deployment_target
- done_reason: stop
- prompt_eval_count: 354
- eval_count: 37

### Phase C — Valid Dependency / Resume
SHA-256:
8122eb721b9344f862efbb491a383c3766639df884dbe0841bec693721a998d6

Result:
CLOSED-PASS

Observed response:
- STATUS: RESUME
- dependency CLOSED
- updated target: sandbox-b
- dry-run handoff resumed for sandbox-b
- done_reason: stop
- prompt_eval_count: 374
- eval_count: 37

## Behavioral Result

| Phase | Linux Mistral | Windows Mistral |
|---|---|---|
| Phase A — STOP | PASS | PASS |
| Phase B — Reject irrelevant delta | PASS | PASS |
| Phase C — Close dependency / Resume | PASS | PASS |

Windows replication result: 3 / 3 PASS.

The Windows run reproduced the same strict dependency-gated behavior observed
in the Linux reference run using byte-identical frozen fixtures.

## GPU Telemetry

File:
gpu_telemetry.csv

Samples:
873 data rows plus CSV header.

Observed maximums:
- GPU temperature: 60 C
- GPU utilization: 98 %
- VRAM used: 20346 MiB
- Power draw: 317.45 W

Telemetry SHA-256:
544F531FFAF50C6B44445BC8B2EC42D9916359CF1C71656DB24B6F128C249BE7

No claim of long-term hardware safety or generalized performance superiority
is made from this telemetry.

## Evidence Integrity

The experiment preserves:
- Windows environment snapshot
- continuous GPU telemetry
- three byte-identical frozen Linux/Windows fixtures
- three raw Mistral run records
- SHA-256 evidence manifest

Authoritative manifest:
LE-WINDOWS-PORTABILITY-001_HASHES.txt

## Established Within Tested Scope

This experiment establishes that the tested Little Engine dependency-gated
behavior reproduced from Linux to Windows under the controlled configuration:

same RTX 3090
+ same Mistral model artifact
+ same Ollama runtime family
+ same frozen fixture bytes
+ same core generation settings
+ operating-system change

The observed workflow reproduced:

verified state
-> missing dependency
-> STOP
-> irrelevant delta rejected
-> dependency remains OPEN
-> requested dependency supplied
-> dependency CLOSED
-> execution RESUMED

## Deferred / Out of Scope

This experiment does not establish:
- universal operating-system portability
- universal model agnosticism
- portability across all hardware
- portability across all runtimes
- production readiness
- generalized latency superiority
- generalized compute or energy savings
- generalized cost reduction
- commercial viability
- patent novelty or validity

Those require separate experiment IDs.

## Final Disposition

LE-WINDOWS-PORTABILITY-001 is CLOSED.

The defined Linux-to-Windows portability question has been answered within
the tested scope. Future Codex/provider portability work will proceed under
a separate experiment ID and does not reopen this experiment.