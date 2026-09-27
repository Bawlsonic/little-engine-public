# Little Engine — Architecture

**Project:** Little Engine  
**Document:** ARCHITECTURE.md  
**Status:** Experimental / evolving  
**Evidence standard:** Proof, not claims.

---

## 1. Purpose

Little Engine is an experimental external context-management architecture for AI systems.

Its central design goal is to avoid repeatedly reconstructing or resending an entire project, dataset, or conversation state when a task only depends on a smaller verified subset.

The working principle is:

> **Little Engine isn't about giving AI less information. It's about giving AI the right information at the right time.**

The architecture is built around:

- verified external state
- relevant change deltas
- dependency-on-demand
- narrow source retrieval
- state verification
- controlled state supersession
- explicit dependency closure

The current implementation maturity is **manual proof of concept**. Automated middleware is a future engineering target, not a completed capability.

---

## 2. Core State Model

A compact model for context evolution is:

```text
Context(t+1)
=
Verified Context(t)
+
Relevant Change Delta
+
Requested Dependency Delta
```

At a higher level:

```text
STATE_n
+
Δ
→ task
→ missing dependency?
→ Δ_dependency
→ result
→ STATE_n+1
```

The model should not be expected to reconstruct unrelated prior state from memory, inference, or broad project rescanning when the controller can instead supply verified state explicitly.

---

## 3. Major Components

Little Engine can be understood as a set of cooperating responsibilities.

### 3.1 Verified State Store

The Verified State Store contains information that has already been established as correct and relevant.

Examples may include:

- known project state
- validated configuration
- prior confirmed outputs
- authoritative source values
- object identifiers
- file fingerprints
- prior accepted analysis state
- dependency-resolution results

A verified state entry should ideally include provenance.

Conceptually:

```text
VerifiedStateItem
├── value
├── source
├── timestamp/version
├── integrity fingerprint
├── scope
└── verification status
```

The architecture should distinguish:

- **verified**
- **unverified**
- **stale**
- **superseded**
- **contradicted**

These states should not be silently collapsed into one another.

---

### 3.2 Relevant Delta

A Relevant Delta represents new or changed information since the last verified state.

The controller should attempt to provide only changes that matter to the current task.

Examples:

```text
File changed
Configuration changed
User requirement changed
Telemetry window changed
New dependency returned
Prior assumption invalidated
```

The goal is not arbitrary context reduction.

The goal is **task-relevant context reduction while preserving correctness**.

---

### 3.3 Task Packet

A task packet combines the minimum context required to begin execution.

Conceptually:

```text
TASK
VERIFIED STATE
RELEVANT DELTA
CONSTRAINTS
INSUFFICIENCY RULE
EXECUTION RULE
REPORTING RULE
```

A model receiving this packet should be instructed to treat verified state as authoritative within the packet's defined scope.

---

### 3.4 Insufficiency Gate

The Insufficiency Gate is one of the most important behaviors in the architecture.

If required context is missing, the model should:

```text
STOP
DO NOT GUESS
DO NOT FABRICATE
IDENTIFY THE MISSING DEPENDENCY
EXPLAIN WHY IT IS NEEDED
REQUEST THE MINIMUM SUFFICIENT ADDITIONAL STATE
```

This converts missing context from an implicit failure mode into an explicit protocol event.

A simplified transition is:

```text
TASK
→ context sufficient?
    ├── YES → execute
    └── NO  → request dependency
```

---

### 3.5 Dependency Request

A Dependency Request identifies the smallest missing piece of information required to continue.

An ideal request is:

- narrow
- explicit
- source-oriented
- task-specific
- bounded in scope

Poor dependency request:

```text
Send the whole project.
```

Better dependency request:

```text
Provide the 60-second telemetry window from 16:12:00–16:13:00
for GPU utilization and GPU temperature only.
```

The architecture should reward specificity.

---

### 3.6 Dependency Resolver

The Dependency Resolver takes a model request and obtains the requested information from an authoritative source.

Potential sources include:

- local files
- databases
- repositories
- telemetry stores
- application state
- APIs
- human-provided evidence

The resolver should not automatically expand scope beyond the request unless necessary.

Conceptually:

```text
DependencyRequest
→ identify authoritative source
→ retrieve bounded slice
→ verify
→ package as Δ_dependency
→ return to model
```

---

### 3.7 Integrity Layer

The Integrity Layer tracks whether state or retrieved dependencies have changed.

Potential mechanisms include:

- SHA256 fingerprints
- chunk hashes
- file hashes
- object version IDs
- timestamps
- deterministic identifiers

Example:

```text
File A
SHA256: abc123...

Chunk 4
SHA256: def456...
```

Hashing establishes integrity, not confidentiality.

The design principle is:

> **Integrity by hashing, confidentiality by encryption, privacy by minimizing exposure.**

---

### 3.8 State Cross-Check

When a dependency overlaps previously verified state, the controller or model should compare the overlap.

Possible results:

```text
MATCH
CONTRADICTION
PARTIAL MATCH
UNVERIFIABLE
```

A match supports carry-forward.

A contradiction should not be silently merged.

Conceptually:

```text
Existing STATE_n
+
Δ_dependency
→ overlap?
    ├── none → append relevant new state
    ├── match → reinforce state
    └── conflict → stop / resolve contradiction
```

---

### 3.9 State Supersession

New evidence may materially change a prior conclusion.

Little Engine should preserve that history rather than silently overwrite it.

Example:

```text
STATE_0:
100% GPU sample observed
duration unknown

Δ_dependency:
1 Hz source window

STATE_1:
100% sample was part of a 2–3 second burst
```

STATE_1 supersedes the relevant interpretation in STATE_0 while preserving the historical chain.

This creates an auditable state transition.

---

### 3.10 Dependency Closure

Once the requested uncertainty is resolved, the system should explicitly mark the dependency closed.

Example:

```text
Dependency: GPU burst duration
Status: CLOSED
Additional state required: NO
```

Explicit closure prevents unnecessary repeated retrieval.

---

## 4. End-to-End Flow

The target Little Engine loop is:

```text
┌─────────────────────┐
│  Verified State     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Relevant Delta      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Task Packet         │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Model Evaluation    │
└──────────┬──────────┘
           │
     ┌─────┴─────┐
     │ sufficient?│
     └─────┬─────┘
       YES │ NO
           │
     ▼     ▼
 Execute   Request
 Task      Dependency
     │         │
     │         ▼
     │   Dependency Resolver
     │         │
     │         ▼
     │   Verified Δ_dependency
     │         │
     │         ▼
     │   Cross-check State
     │         │
     └─────────┘
           │
           ▼
┌─────────────────────┐
│ Updated Result      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ STATE_n+1           │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Dependency Closure  │
└─────────────────────┘
```

---

## 5. Manual Prototype vs Future Automated System

### Current Manual POC

Today, some controller responsibilities are performed manually.

These include:

- selecting verified state
- preparing the task packet
- interpreting dependency requests
- locating authoritative source data
- extracting the requested slice
- checking returned evidence
- updating experiment records

This means the current proof is of the **protocol behavior**, not a finished software stack.

### Future Automated Target

A future Little Engine controller could automate:

```text
Task intake
↓
State lookup
↓
Relevant-state selection
↓
Fingerprint validation
↓
Prompt/context packet assembly
↓
Model invocation
↓
Dependency-request parsing
↓
Targeted source retrieval
↓
Delta verification
↓
State update
↓
Result return
↓
Audit log
```

---

## 6. Candidate Internal Modules

A future implementation may separate responsibilities into modules such as:

```text
little_engine/
├── state/
│   ├── verified_state_store
│   ├── state_versioning
│   └── supersession
│
├── integrity/
│   ├── file_hashing
│   ├── chunk_hashing
│   └── fingerprint_validation
│
├── selection/
│   ├── task_relevance
│   └── context_builder
│
├── dependencies/
│   ├── request_parser
│   ├── source_router
│   ├── retriever
│   └── resolver
│
├── validation/
│   ├── overlap_check
│   ├── contradiction_detection
│   └── sufficiency_check
│
├── orchestration/
│   ├── controller
│   ├── model_adapter
│   └── execution_loop
│
└── audit/
    ├── event_log
    ├── experiment_trace
    └── result_record
```

This is a design direction, not a claim that all modules currently exist.

---

## 7. State Lifecycle

A useful state lifecycle may be:

```text
UNVERIFIED
    ↓
VERIFIED
    ↓
ACTIVE
    ↓
SUPERSEDED
```

With alternate transitions:

```text
VERIFIED
→ STALE

VERIFIED
→ CONTRADICTED

UNVERIFIED
→ REJECTED
```

State should always retain provenance where practical.

---

## 8. Dependency Lifecycle

Dependencies may follow:

```text
IDENTIFIED
→ REQUESTED
→ RETRIEVED
→ VERIFIED
→ CONSUMED
→ CLOSED
```

Failure paths may include:

```text
REQUESTED
→ NOT FOUND

RETRIEVED
→ INVALID

VERIFIED
→ CONTRADICTORY
```

Each stage should be auditable.

---

## 9. Security and Privacy Model

Little Engine's privacy advantage, if validated, would come primarily from **minimizing unnecessary exposure**.

Potential principles:

- do not send unrelated files
- do not send unrelated project history
- retrieve only required source slices
- preserve sensitive data locally when possible
- separate identity data from task data
- avoid publishing real private telemetry or workplace data
- prefer synthetic data for public demonstrations

Hashing provides integrity checking.

Encryption provides confidentiality.

Data minimization reduces the amount of information exposed in the first place.

These controls solve different problems and should not be conflated.

---

## 10. Relationship to Retrieval Systems

Little Engine may overlap conceptually with ideas found in:

- retrieval
- state machines
- caches
- agent memory
- event sourcing
- incremental computation
- dependency graphs

However, the current Little Engine framing is specifically centered on:

**externally verified state + task-relevant deltas + explicit dependency-on-demand.**

The project should not claim superiority over RAG, retrieval systems, or agent memory architectures without direct comparative evidence.

---

## 11. Experimental Validation

The first formal experiment package is:

```text
experiments/telemetry_closed_loop/
```

Experiment:

**LE-TCL-001**

Observed sequence:

```text
Broad/raw path
→ context overflow

Little Engine path
→ verified compact state
→ insufficiency detection
→ narrow dependency request
→ targeted retrieval
→ cross-check
→ revised result
→ dependency closure
```

That experiment supports the architecture as technically testable.

It does not prove universal behavior.

---

## 12. Auditability

A Little Engine execution should ideally produce an audit record containing:

```text
Task ID
State version
State items supplied
Relevant delta supplied
Model identity
Context configuration
Dependency requests
Sources queried
Returned dependencies
Verification result
State changes
Final result
Closure status
Timestamps
```

This enables later review of:

- what the model knew
- what it did not know
- what it requested
- what was supplied
- what changed
- why the final result changed

---

## 13. Failure Handling

The architecture should favor explicit failure over silent guessing.

Examples:

### Missing dependency

```text
STOP
Required dependency not supplied.
```

### Contradictory state

```text
STOP
New evidence conflicts with verified state.
Resolve contradiction before continuation.
```

### Source unavailable

```text
STOP
Authoritative source could not be retrieved.
```

### Invalid integrity check

```text
STOP
Source fingerprint does not match expected state.
```

Fail-closed behavior is a design feature.

---

## 14. Design Principles

### Minimum Sufficient Verified Context

Do not optimize for smallest possible context at the expense of correctness.

Optimize for the smallest **sufficient and verified** context.

### Explicit Uncertainty

Missing information should become a visible protocol event.

### Evidence Before State Change

State should not change merely because the model inferred something.

### Narrow Retrieval

Retrieve the smallest useful dependency that resolves the current uncertainty.

### Preserve History

Supersede prior state; do not silently erase it.

### Close Dependencies

Explicitly stop retrieving once the uncertainty is resolved.

### Separate Mechanism from Concept

Hashes, caching, and deltas are mechanisms.

Verified-state context governance is the larger architecture under investigation.

---

## 15. Current Boundaries

Little Engine is currently:

- experimental
- manually orchestrated
- supported by a small number of controlled tests
- not production middleware
- not benchmarked at scale
- not validated across broad model families
- not proven commercially viable

Those boundaries should remain explicit in documentation.

---

## 16. Future Validation Goals

Future experiments should evaluate:

- repeatability across models
- repeatability across interfaces
- automated dependency parsing
- automated source retrieval
- contradiction handling
- state staleness
- state-version growth
- multi-file repositories
- agent workflows
- token usage
- latency
- compute utilization
- memory utilization
- power utilization
- failure cases
- adversarial or incorrect dependency requests

A meaningful future comparison should distinguish:

```text
raw/broad context
vs
standard retrieval
vs
Little Engine
```

using the same task and evidence source where practical.

---

## 17. Current Architectural Classification

```text
System type:
Experimental external context/state controller

Primary abstraction:
Verified State

Change mechanism:
Relevant Delta

Missing-context mechanism:
Dependency-on-Demand

Verification mechanism:
Source provenance + overlap checks + integrity fingerprints

State evolution:
Versioned / superseding

Controller:
Manual today

Automation:
Future target

Evidence level:
Proof of concept
```

---

## Closing

Little Engine is exploring whether AI workflows can become more reliable and efficient by moving context responsibility outside the model and treating context as controlled, versioned, verifiable state rather than an ever-growing prompt history.

The architecture is still early.

The important requirement is that each future claim remains tied to reproducible evidence.

> **Proof, not claims.**
