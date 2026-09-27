# Current Validation Notice

> The authoritative current project status is maintained in [CURRENT_STATUS.md](CURRENT_STATUS.md).
> Sections below preserve the project's historical validation progression and may describe earlier evidence boundaries.

# Little Engine - Validation Framework

**Project:** Little Engine  
**Document:** VALIDATION.md  
**Status:** Active experimental standard  
**Evidence principle:** Proof, not claims.

---

## 1. Purpose

This document defines how Little Engine experiments should be designed, recorded, interpreted, and promoted into stronger claims.

The goal is to prevent the project from confusing:

- interesting behavior
- repeatable behavior
- comparative advantage
- production capability
- commercial viability

Each claim should be tied to evidence.

---

## 2. Core Rule

Little Engine follows one rule above all others:

> **Proof, not claims.**

A result should not be promoted beyond what the experiment actually demonstrated.

Examples:

A single successful run may support:

```text
Observed in this experiment.
```

It does not automatically support:

```text
Works reliably.
```

A successful manual workflow may support:

```text
Manual closed-loop behavior demonstrated.
```

It does not support:

```text
Automated middleware demonstrated.
```

A context-overflow control may support:

```text
The control request exceeded this configured context window.
```

It does not support:

```text
Traditional retrieval cannot handle this task.
```

---

## 3. Evidence Levels

Little Engine uses the following evidence ladder.

### Level 0 - Hypothesis

An idea or expected behavior with no direct experimental support yet.

Example:

```text
Little Engine may reduce redundant context.
```

### Level 1 - Observation

A behavior occurred in one experiment.

Example:

```text
Qwen requested a narrowly scoped dependency in LE-TCL-001.
```

### Level 2 - Reproduction

The same behavior is reproduced under the same or closely controlled conditions.

Example:

```text
The same STOP -> dependency -> resume behavior occurred across repeated runs.
```

### Level 3 - Cross-Condition Validation

The behavior survives meaningful variation.

Examples:

- different prompts
- different datasets
- different model runs
- different context sizes
- different interfaces

### Level 4 - Comparative Validation

Little Engine is compared against a defined alternative under controlled conditions.

Examples:

```text
Broad context vs Little Engine
Standard retrieval vs Little Engine
Single-agent workflow vs Little Engine
```

Metrics should be defined before the comparison where practical.

### Level 5 - Automated System Validation

The controller behavior is performed by Little Engine software rather than manual orchestration.

This requires evidence for:

- automatic state lookup
- context selection
- dependency parsing
- targeted retrieval
- verification
- state update
- closure

### Level 6 - Operational Validation

The system is evaluated over sustained real workflows.

Possible criteria include:

- reliability
- recovery from failure
- stale-state handling
- contradictory-state handling
- repeatability
- observability
- security
- resource use

### Level 7 - Production Readiness

This level requires substantially more than a successful prototype.

Potential requirements include:

- robust automated tests
- defined failure modes
- security review
- reproducible deployment
- monitoring
- rollback
- documentation
- privacy boundaries
- performance characterization

Little Engine is **not currently at this level**.

---

## 4. Claim Categories

Every major conclusion should fit into one of four categories.

### OBSERVED

Directly demonstrated by evidence.

Example:

```text
The assembled request contained 32,918 tokens and exceeded the configured 32,768-token context.
```

### SUPPORTED

A cautious interpretation reasonably supported by observed evidence.

Example:

```text
The result supports further investigation of dependency-on-demand as a context-management strategy.
```

### HYPOTHESIS

A plausible explanation or future expectation requiring additional testing.

Example:

```text
Automating the controller may reduce redundant context retrieval.
```

### NOT ESTABLISHED

Important conclusions that the experiment does not justify.

Example:

```text
Exact compute savings are not established.
```

Each formal experiment results file should include these categories where relevant.

---

## 5. Experiment Identification

Formal experiments should receive stable IDs.

Recommended format:

```text
LE-[CATEGORY]-[NUMBER]
```

Examples:

```text
LE-TCL-001
LE-CTX-001
LE-DEP-001
LE-AGT-001
LE-PERF-001
```

Possible categories:

```text
TCL  Telemetry Closed Loop
CTX  Context Management
DEP  Dependency Resolution
AGT  Agent Workflow
PERF Performance
INT  Integrity
SEC  Security
REP  Reproduction
CMP  Comparative
```

Experiment IDs should not be reused.

---

## 6. Minimum Experiment Package

A formal experiment should preserve:

```text
experiment/
â”œâ”€â”€ README.md
â”œâ”€â”€ raw/
â”œâ”€â”€ prompts/
â”œâ”€â”€ responses/
â”œâ”€â”€ screenshots/
â””â”€â”€ results/
```

Where applicable:

### raw/

Authoritative source inputs.

Examples:

- CSV
- JSON
- logs
- source files
- synthetic datasets

### prompts/

Exact or reconstructed prompt packets.

If wording is reconstructed, label it clearly.

### responses/

Model outputs or conservative response records.

### screenshots/

High-value visual evidence only.

Avoid collecting large numbers of redundant screenshots.

### results/

Calculated results, conclusions, limitations, and classification.

### README.md

The experiment index and narrative.

---

## 7. Exact vs Reconstructed Evidence

Evidence must distinguish exact preservation from reconstruction.

Use labels such as:

```text
EXACT TRANSCRIPTION
```

or:

```text
RECONSTRUCTED FROM EXPERIMENT RECORD
```

Do not present reconstructed prompt wording as exact if the original text was not preserved.

Screenshots, raw logs, and exported model responses should take precedence over later summaries.

---

## 8. Control Requirements

A strong experiment should include a control when the hypothesis is comparative.

Examples:

```text
CONTROL A:
Broad/raw context

TEST B:
Little Engine context
```

The control and test should use the same:

- task
- source data
- model when possible
- context configuration
- relevant runtime settings

If settings differ, document the difference.

---

## 9. Variable Discipline

Change as few variables as practical.

Before testing, identify:

### Independent variable

The intentional change.

Example:

```text
Context-delivery method
```

### Dependent variables

The outcomes measured.

Examples:

```text
Task completion
Context size
Latency
Correctness
Dependency count
Files inspected
```

### Controlled variables

Settings held constant.

Examples:

```text
Model
Quantization
Context limit
Source data
Task
Hardware
```

If a variable cannot be controlled, document it.

---

## 10. Success Criteria

Define success before interpreting the result.

Example for a dependency experiment:

```text
PASS if:
1. Model stops when required state is absent.
2. Model identifies a specific dependency.
3. Returned dependency is sufficient.
4. Prior verified state is preserved.
5. Result updates correctly.
6. Dependency closes.

FAIL if:
1. Model fabricates missing state.
2. Model requests unrelated broad context.
3. New evidence contradicts prior state and is silently merged.
4. Model continues without required dependency.
```

This reduces post-hoc interpretation.

---

## 11. Failure Is Evidence

Failed tests should be preserved when they reveal meaningful behavior.

A failure may expose:

- weak insufficiency rules
- over-broad retrieval
- prompt sensitivity
- state corruption
- contradiction handling problems
- incorrect dependency requests
- context overflow
- model inconsistency

Do not delete failed experiments merely because they are inconvenient.

A failed experiment can be more valuable than an easy success.

---

## 12. Reproduction Standard

A behavior should not be described as reliable after one success.

For reproduction, record:

```text
Run count
Pass count
Fail count
Prompt version
Model version
Runtime version
Context size
Relevant configuration changes
```

Example:

```text
5 runs
4 pass
1 fail
```

is more informative than:

```text
Works.
```

---

## 13. Comparative Testing

Future comparisons should ideally test:

```text
A - broad/raw context
B - standard retrieval
C - Little Engine
```

under the same task.

Useful metrics may include:

- task success
- factual correctness
- context tokens
- retrieved bytes
- files inspected
- dependency count
- time to first result
- total latency
- model evaluation time
- CPU utilization
- GPU utilization
- VRAM use
- RAM use
- power draw
- number of retries
- hallucination / unsupported-claim rate

Not every experiment needs every metric.

---

## 14. Efficiency Claims

Efficiency claims require actual measurement.

Do not infer:

```text
faster
cheaper
lower power
lower compute
fewer tokens
```

merely because less source data appears in a prompt.

To make those claims, collect the relevant metric.

Example:

```text
Context size decreased from X to Y.
```

supports a context-size claim.

It does not automatically prove:

```text
Total compute decreased by Z%.
```

---

## 15. Accuracy Claims

A shorter context is not automatically more accurate.

A longer context is not automatically more accurate.

Accuracy testing requires a known evaluation target.

Potential methods:

- expected-answer sets
- known source truth
- deterministic transformations
- independent human review
- automated checks for exact fields
- repeated scoring

Accuracy superiority should not be claimed until directly compared.

---

## 16. Dependency Quality

Dependency requests should be evaluated for quality.

Possible measures:

### Specificity

Did the model request exactly what it needed?

### Scope

Did it ask for excessive unrelated context?

### Sufficiency

Did the returned dependency actually resolve the question?

### Closure

Did the model stop requesting additional context once resolved?

### Stability

Would repeated runs request roughly the same evidence?

These may become formal benchmark dimensions later.

---

## 17. State Integrity Validation

Future experiments should test:

- unchanged-state carry-forward
- stale state
- contradictory state
- partial overlap
- missing source
- changed hashes
- incorrect source mapping
- duplicate dependencies

The system should not silently merge contradictions.

Expected behavior for contradictions should be explicit.

---

## 18. Model Portability

Little Engine should not be described as model-independent until tested across multiple model families.

Future portability tests may include:

- local dense models
- local mixture-of-experts models
- cloud models
- different context lengths
- different reasoning settings

A result from one model establishes behavior for that model/configuration only.

---

## 19. Interface Portability

Similarly, behavior may differ across:

- LM Studio
- Open WebUI
- direct API invocation
- agent frameworks
- CLI interfaces
- custom middleware

Retrieval behavior and prompt packing can materially affect results.

Document the interface.

---

## 20. Manual vs Automated Boundary

Every experiment should explicitly state the controller type.

Allowed classifications:

```text
Manual
Partially Automated
Automated
```

A manual experiment may validate the protocol.

It does not validate the automation.

This distinction should remain visible in all public-facing documentation.

---

## 21. Security and Privacy Validation

Future security-oriented tests should distinguish:

### Integrity

Can we detect changed source/state?

### Confidentiality

Can unauthorized parties read protected data?

### Privacy

Can unnecessary sensitive data be avoided entirely?

Potential validation areas:

- hash verification
- encryption at rest
- encryption in transit
- local-only source processing
- least-context retrieval
- logging boundaries
- secret redaction
- public-demo synthetic datasets

Do not claim privacy solely because a model runs locally.

---

## 22. Public Evidence Standard

Before public release, every experiment should be reviewed for:

- personal information
- workplace information
- secrets
- credentials
- proprietary source code
- confidential logs
- identifying metadata

Public examples should use synthetic or intentionally releasable data unless explicit permission exists.

---

## 23. Experiment Freeze Rule

When an experiment package is complete:

```text
FREEZE
```

Do not continue editing it casually.

Corrections should be versioned.

If a factual error is found:

1. preserve the original
2. document the issue
3. create a corrected revision
4. explain the change

This preserves research history.

---

## 24. Current Formal Evidence

### LE-TCL-001

**Name:** Telemetry Closed Loop  
**Date:** September 12, 2026  
**Result:** PASS  
**Validation:** Manual Proof of Concept

Established in that experiment:

- direct raw retrieval exceeded the configured context
- fail-closed behavior occurred
- verified compact state was usable
- the model requested a narrow dependency
- a targeted source delta resolved the question
- overlapping state was cross-checked
- the prior conclusion was superseded
- the dependency explicitly closed

Not established:

- automated middleware
- universal portability
- exact efficiency savings
- accuracy superiority
- production readiness

---

## 25. Promotion Rules

A claim should be promoted only when evidence justifies it.

Example progression:

```text
Hypothesis:
Dependency-on-demand may reduce unnecessary context.

Observation:
Qwen requested a one-minute dependency in LE-TCL-001.

Reproduction:
Multiple controlled runs reproduce narrow dependency requests.

Cross-condition:
Behavior survives different datasets/models.

Comparative:
Little Engine outperforms defined alternatives on measured criteria.

Automated validation:
Software performs the loop without manual orchestration.

Operational:
System remains reliable across sustained real workflows.
```

Do not skip levels for marketing language.

---

## 26. Current Project Validation Status

```text
Manual closed-loop behavior:
DEMONSTRATED

Fail-closed insufficiency behavior:
DEMONSTRATED

Dependency-on-demand:
DEMONSTRATED

State cross-check:
DEMONSTRATED

State supersession:
DEMONSTRATED

Dependency closure:
DEMONSTRATED

Automated middleware:
NOT YET DEMONSTRATED

Cross-model portability:
NOT YET ESTABLISHED

Comparative efficiency:
NOT YET ESTABLISHED

Production readiness:
NOT ESTABLISHED
```

---

## Closing

Little Engine should become stronger by accumulating reproducible evidence, not by expanding the language around early results.

The purpose of this framework is to make it difficult for the project to accidentally overstate what has been shown.

> **Build the evidence first. Let the claims follow.**

**Proof, not claims.**
