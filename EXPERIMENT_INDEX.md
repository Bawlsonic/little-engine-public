# Little Engine - Experiment Index

This index describes the evidence available in LE-PUBLIC-RC1.

For publication provenance and sanitization policy, see `PUBLIC_PROVENANCE.md`.

For known discrepancies and evidence limitations, see `ERRATA_AND_LIMITATIONS.md`.

## Evidence Status Vocabulary

- OBSERVED - directly inspectable in preserved evidence.
- SUPPORTED - supported within the specifically tested configurations.
- REPORTED - experiment result documented, but supporting raw provenance is incomplete.
- BOUNDARY - preserved result that did not conform to the tested contract or could not establish the intended behavior.
- NOT ESTABLISHED - not demonstrated by the current evidence.

## Experiment Matrix

| Experiment | Environment / Surface | Evidence | Result / Status |
|---|---|---|---|
| LE-LINUX-CLOSEOUT-001 | Linux, RTX 3090, Ollama | Raw JSON, telemetry, environment snapshot, manifests | CLOSED; mixed model conformance and preserved boundaries |
| LE-WINDOWS-PORTABILITY-001 | Windows, RTX 3090, Ollama | Raw JSON, telemetry, environment snapshot, fixtures | CLOSED; 3/3 recorded PASS |
| LE-CODEX-PORTABILITY-001 | Codex hosted model configurations | Expected contracts and narrative result documentation | CLOSED; reported PASS results with incomplete raw-output provenance |
| LE-CLAUDE-CODE-PORTABILITY-001 | Claude Code desktop, Opus 5, High | Baseline, annotated phase records, closeout, verifiable manifest | CLOSED; Phase A/B/C 3/3 recorded PASS on Run 01 |
| LE-TCL-001 | Telemetry closed-loop checkpoint | Source telemetry, prompts, responses, screenshots | Historical behavioral evidence; see temperature erratum |
| LE-NVIDIA-001 | Linux / NVIDIA environment checkpoint | Narrative/setup evidence | Historical checkpoint |
| LE-EFFICIENCY-001 | Controlled efficiency experiment | Planning documentation | NOT STARTED; no generalized efficiency result |

## Core Tested Sequence

The portability experiments evaluate a dependency-gated sequence:

1. Begin from VERIFIED STATE.
2. Encounter a required missing dependency.
3. STOP rather than infer the missing value.
4. Request the minimum missing dependency.
5. Reject an irrelevant delta while keeping the dependency OPEN.
6. Accept the requested dependency when supplied.
7. CLOSE the dependency.
8. RESUME the requested task using the updated verified state.

The A/B/C fixtures test response conformance to that sequence.

They do not independently establish autonomous state-store mutation, autonomous dependency discovery, or real deployment execution.

## Local Model Evidence

The local evidence includes multiple model families and preserved non-conforming results.

A successful result for one model or configuration is not generalized to all local models.

Preserved failures and boundaries remain part of the evidence.

## Hosted / Frontier-Model Evidence

The evidence includes tested hosted model execution surfaces through Codex and Claude Code.

This supports investigation of behavioral portability beyond the original local environment.

It does not establish universal provider portability or universal frontier-model portability.

## Evidence Quality Notes

### Linux

The Linux package contains raw model records and telemetry.

Some historical wording and manifest details require the qualifications documented in `ERRATA_AND_LIMITATIONS.md`.

### Windows

The Windows package contains the A/B/C source fixtures, raw model records, telemetry, environment information, and closeout documentation.

Publication copies contain explicit redactions of identifying environment metadata.

### Codex

The Codex package reports successful A/B/C results across the tested configurations.

Complete actual run outputs were not preserved in the committed experiment package.

Public claims therefore identify these as reported results rather than independently reconstructable raw runs.

### Claude Code

The Claude package preserves baseline and annotated A/B/C records.

The tested configuration recorded 3/3 PASS on Run 01.

Native per-turn provider telemetry and the bundled desktop runtime version were not captured.

## What the Current Evidence Supports

Within tested scope, the evidence supports continued investigation of:

- dependency-gated fail-closed behavior
- selective acceptance of relevant dependency information
- rejection of irrelevant dependency deltas
- response-conformance reproduction across named configurations
- portability of the tested behavioral contract across multiple execution environments

## What Is Not Established

The current evidence does not establish:

- universal model agnosticism
- universal provider portability
- universal operating-system portability
- universal frontier-model portability
- generalized efficiency
- production readiness
- commercial viability
- patent novelty or validity

See `ERRATA_AND_LIMITATIONS.md` for the complete evidence-boundary discussion.
