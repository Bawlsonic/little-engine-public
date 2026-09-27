# Little Engine

> **Public validation release candidate:** `LE-PUBLIC-RC1`
>
> This repository is a sanitized derivative of the private authoritative Little Engine experimental archive.
>
> **Start here:** [Experiment Index](EXPERIMENT_INDEX.md) | [Public Provenance](PUBLIC_PROVENANCE.md) | [Errata and Evidence Limitations](ERRATA_AND_LIMITATIONS.md)
>
> Historical sections below preserve earlier project stages and may use terminology superseded by the current evidence index.
> **Current validation state:** [CURRENT_STATUS.md](CURRENT_STATUS.md)
> Historical sections below document earlier project stages.
>
> **Historical display note:** The preserved [NVIDIA checkpoint README](experiments/linux_nvidia_portability/README.md) contains garbled temperature-unit characters at lines 78 and 147. The historical text is retained unchanged.


**Status:** Early proof of concept / experimental  
**Evidence standard:** Proof, not claims.

Little Engine is an experimental approach to AI context management built around a simple question:

> How much context does an AI system actually need to complete the current task reliably?

The project explores **externally maintained verified state**, **relevant change deltas**, and **dependency-on-demand**. Instead of repeatedly supplying an entire project or source dataset, the controller supplies the minimum verified context believed sufficient for the task. If the model determines that required evidence is missing, it must stop, identify the missing dependency, and request only the additional information needed to proceed.

A compact description is:

```text
Context(t+1)
=
Verified Context(t)
+
Relevant Change Delta
+
Requested Dependency Delta
```

The intended interaction pattern is:

```text
STATE_n
+
-> task
-> missing dependency?
-> request delta_dependency
-> supply verified dependency
-> verify / cross-check
-> result
-> STATE_n+1
```

## Core Principle

**Little Engine isn't about giving AI less information. It's about giving AI the right information at the right time.**

The current design emphasizes three ideas:

- **Minimum sufficient verified context**
- **Complex underneath, simple on top**
- **Integrity by hashing, confidentiality by encryption, privacy by minimizing exposure**

## Current Architecture

Little Engine is being explored as an **external verified-state and context-governance system with dependency-on-demand**.

Caching, SHA256 fingerprints, chunk-level hashes, state snapshots, and delta packets are potential implementation mechanisms underneath that architecture. They are not, by themselves, the main concept.

The current proof-of-concept behavior is:

1. Maintain a known verified state outside the model.
2. Supply only task-relevant verified state and changes.
3. Require the model to fail closed when required context is absent.
4. Allow the model to request a narrowly scoped missing dependency.
5. Retrieve that dependency from an authoritative source.
6. Cross-check new evidence against existing verified state.
7. Update the result and state only when supported by verified evidence.
8. Close the dependency when the question is resolved.

## Current Validation Boundary

Little Engine is **not yet automated middleware**.

The strongest validation so far used a **manual controller**. Human orchestration performed the state-management and targeted-retrieval responsibilities that a future Little Engine implementation would need to automate.

Current evidence therefore supports a **manual closed-loop proof of concept**, not a production-ready system.

No claims are currently made for:

- universal model compatibility
- universal superiority over RAG or conventional retrieval
- exact token savings
- exact compute savings
- exact latency savings
- exact power savings
- exact monetary cost savings
- accuracy superiority
- production readiness
- commercial viability
- novelty
- patentability

## Experiment Evidence

Formal experiment packages are stored under:

```text
experiments/
```

### LE-TCL-001 - Telemetry Closed Loop

Location:

```text
experiments/telemetry_closed_loop/
```

**Date:** September 12, 2026  
**Result:** PASS  
**Validation level:** Manual Proof of Concept

LE-TCL-001 tested Little Engine behavior against real hardware telemetry using Qwen3.8-27B through LM Studio and Open WebUI with a configured 32,768-token context.

The control/raw path assembled a 32,918-token request and exceeded the configured context before the requested task could run.

The Little Engine path instead:

```text
Verified state
-> limited truthful analysis
-> unresolved question
-> minimum dependency request
-> targeted source retrieval
-> verified dependency delta
-> cross-check
-> state update
-> updated result
-> dependency closure
```

The model requested a specific one-minute telemetry window rather than the full source dataset, consumed the returned dependency, cross-checked overlapping measurements against prior verified state, revised its earlier conclusion, and explicitly reported that no further dependency was required for the requested resolution.

This result applies to that specific experiment. It is not treated as universal proof.

## Repository Structure

> **Historical layout:** The tree below describes the earlier LE-TCL-001 project stage. See the [Experiment Index](EXPERIMENT_INDEX.md) for the complete RC1 experiment inventory.

Current project organization:

```text
Little Engine/
|-- README.md
`-- experiments/
    `-- telemetry_closed_loop/
        |-- README.md
        |-- raw/
        |-- prompts/
        |-- responses/
        |-- screenshots/
        `-- results/
```

Each experiment should preserve its own evidence boundary.

A formal experiment package should contain, where applicable:

- authoritative raw input
- prompts and dependency packets
- model responses
- screenshots
- calculated or summarized results
- experiment-specific README
- limitations and claim boundaries

## Evidence Integrity

Original source material and direct captured evidence are authoritative.

If a later transcription or summary conflicts with preserved source evidence:

1. Preserve the historical artifact.
2. Document the discrepancy.
3. Defer to the authoritative source.
4. Create a corrected version.
5. Do not silently rewrite historical evidence.

Failed experiments and contradictory results should be preserved rather than discarded when they materially affect interpretation.

## Development Direction

Future work may investigate:

- automated verified-state maintenance
- deterministic source discovery
- SHA256 state and chunk fingerprints
- task-aware context selection
- dependency parsing and routing
- automated targeted retrieval
- state versioning and supersession
- contradiction detection
- dependency closure
- repeatability across local and cloud models
- controlled comparisons against broader-context approaches
- measurable token, latency, compute, and power effects

These are development and research directions, not completed capabilities unless separately demonstrated by evidence.

## Public Release Boundary

> **Historical publication guidance:** The paragraphs below predate the sanitized public derivative. See [Public Provenance](PUBLIC_PROVENANCE.md) for the RC1 publication boundary.

The project should remain private while the implementation and evidence package are still being organized.

If a public repository is created later, public artifacts should use reproducible synthetic or intentionally releasable demo data. Workplace, personal, confidential, or otherwise private source data should not be published.

Public documentation should clearly separate:

- demonstrated behavior
- supported interpretation
- hypothesis
- future design goals

## Current Project Classification

```text
Project: Little Engine
Stage: Early experimental proof of concept
Primary concept: External verified-state/context management
Dependency model: Dependency-on-demand
Strongest evidence: LE-TCL-001
Controller validation: Manual
Automated middleware validation: Not yet
Production readiness: Not established
```

---

> **Proof, not claims.**


