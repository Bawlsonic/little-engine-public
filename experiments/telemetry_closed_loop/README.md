# Little Engine — Telemetry Closed-Loop Experiment

## Experiment Record

**Experiment ID:** LE-TCL-001  
**Date:** September 12, 2026  
**Result:** PASS  
**Validation Level:** Manual Proof of Concept  
**Controller:** Manual  
**Model:** Qwen3.8-27B  
**Model Format:** GGUF Q4_K_M  
**Model Size:** Approximately 17.74 GB  
**Model Runtime:** LM Studio  
**Interface:** Open WebUI  
**Context Limit:** 32,768 tokens  
**Primary GPU:** AMD Radeon RX 7900 XTX — 24 GB VRAM  

---

## Purpose

This experiment tests whether Little Engine can allow an AI model to complete a task using externally maintained verified state and dependency-on-demand, rather than requiring the complete source dataset to remain in the model's working context.

The experiment specifically evaluates whether the model can:

1. Operate from a limited verified state.
2. Detect when required information is missing.
3. Refuse to fabricate missing information.
4. Request a specific missing dependency.
5. Resume after receiving only that dependency.
6. Cross-check new information against previously verified state.
7. Update its conclusion when warranted.
8. Stop when no additional dependency is required.

---

## Core Principle

> Little Engine isn't about giving AI less information. It's about giving AI the right information at the right time.

The working state model is:

`Context(t+1) = Verified Context(t) + Relevant Change Delta + Requested Dependency Delta`

The corresponding closed-loop behavior is:

`STATE₀ → Task → Missing Dependency → Request Δ → Supply Δ → Verify → STATE₁ → Continue/Stop`

---

## Source Dataset

The experiment used real hardware telemetry collected from the local test system.

The source CSV contained:

- 1 summary row without a timestamp.
- 600 timestamped telemetry samples.
- Approximately 10 minutes of telemetry.
- Approximately 1-second sampling intervals.

Timestamped range:

`2026-09-12 16:03:00.315 → 2026-09-12 16:12:59.081`

Available telemetry included:

- GPU utilization
- GPU clock
- GPU board power
- GPU temperature
- GPU hotspot temperature
- GPU fan
- GPU voltage
- GPU memory utilization
- GPU memory clock
- GPU memory temperature
- CPU utilization
- System memory utilization

CPU temperature was **not present** in the source dataset.

System memory was represented as **utilization percentage**, not GB.

---

## Control — Direct Raw Retrieval

The original hardware telemetry CSV was attached directly through Open WebUI.

Open WebUI assembled a request of approximately:

**32,918 tokens**

The configured model context limit was:

**32,768 tokens**

Difference:

**Approximately 150 tokens over the configured context limit**

Result:

**FAIL — the request exceeded the available context before the analysis task could run.**

This establishes the specific failure condition used for comparison.

It does **not** establish that all conventional retrieval methods would fail. It establishes only that this direct raw-file retrieval path exceeded the configured context limit under this specific test configuration.

---

## Little Engine Path

A fresh model session received the Little Engine protocol instead of the complete raw telemetry dataset.

The protocol supplied:

- Task scope
- Verified state
- Relevant context
- Constraints
- An explicit insufficiency rule
- Dependency-on-demand behavior

The insufficiency rule required the model to stop rather than guess when necessary information was missing.

### Initial Insufficiency Test

The first telemetry packet contained placeholders rather than actual verified measurements.

The model correctly detected that:

- Timestamps were missing.
- Sample counts were missing.
- Summary statistics were not populated.
- No usable time-series measurements had been supplied.

The model refused to fabricate telemetry and requested additional evidence.

**Result: PASS — fail-closed behavior observed.**

---

## Initial Verified Dependency Delta

A compact three-point co-timestamped telemetry delta was supplied.

### Point 1

**Timestamp:** 16:03:00.315

- GPU utilization: 6%
- GPU temperature: 44°C
- CPU utilization: 6.34%
- System memory utilization: 38.90%

### Point 2

**Timestamp:** 16:12:14.081

- GPU utilization: 100%
- GPU temperature: 46°C
- CPU utilization: 8.55%
- System memory utilization: 35.26%

### Point 3

**Timestamp:** 16:12:59.081

- GPU utilization: 5%
- GPU temperature: 44°C
- CPU utilization: 4.82%
- System memory utilization: 35.60%

CPU temperature remained unavailable because it was absent from the source data.

---

## Model Behavior From the Three-Point State

Using only the supplied verified evidence, the model:

1. Accepted the three co-timestamped points as sufficient for a limited visualization.
2. Excluded CPU temperature because it was unavailable.
3. Detected that system-memory values represented percentages rather than GB.
4. Refused to treat unverified placeholder summary statistics as authoritative.
5. Recomputed statistics using only the three supplied points.
6. Explicitly identified the large unobserved time gap between measurements.
7. Did not claim that connecting lines represented measured intermediate values.
8. Identified the 100% GPU utilization measurement as an unresolved question.

The model then requested additional evidence.

---

## Dependency Request

The model requested a narrowly scoped telemetry window:

**Time range:** 16:12:00 → 16:13:00  
**Sampling:** Approximately 1 Hz  
**Required fields:**

- GPU utilization
- GPU temperature

CPU temperature was requested only if available.

The model explicitly indicated that it did not require the complete original CSV or the preceding telemetry window to resolve the immediate question.

This became the experiment's requested dependency delta.

---

## Targeted Retrieval

The requested time range was retrieved from the original source dataset.

The dependency delta contained:

**60 real co-timestamped samples**

Only these fields were supplied:

- Timestamp
- GPU utilization
- GPU temperature

CPU temperature was explicitly reported as unavailable.

The complete original CSV was **not** resent to the model.

The model was instructed to:

- Resume from the previously verified state.
- Use the supplied dependency only where required.
- Avoid reconstructing omitted telemetry.
- Cross-check overlapping measurements.
- Update its previous conclusion only if the new evidence warranted it.

---

## Dependency Resolution

After receiving the requested 60-sample dependency delta, the model:

1. Cross-checked overlapping timestamps against the previously verified three-point state.
2. Confirmed that the overlapping measurements matched.
3. Carried the prior verified state forward.
4. Determined that the 100% GPU utilization sample was not isolated.
5. Identified multiple brief high-utilization episodes.
6. Updated the visualization using the higher-resolution dependency data.
7. Updated its previous conclusion.
8. Determined that no additional dependency was required to answer the burst-resolution question.

This completed the manual Little Engine closed loop:

`Verified State → Uncertainty → Dependency Request → Targeted Retrieval → Verified Delta → Cross-Check → State Update → Updated Result → Dependency Closed`

---

## Resolved GPU Event

The 100% GPU utilization measurement at:

`16:12:14.081`

was not an isolated single-sample spike.

It began a three-sample high-utilization sequence:

`100% → 96% → 88%`

across approximately:

`16:12:14.081 → 16:12:16.086`

The elapsed time between the first and last samples was:

**2.005 seconds**

At the available approximately 1 Hz sampling rate, this is consistent with roughly **2–3 seconds of sustained high GPU utilization**.

Other high-utilization events were also present within the requested 60-second window.

---

## Thermal Response

GPU temperature was approximately:

**45°C baseline**

At the start of the primary burst:

**46°C**

Peak observed temperature:

**51°C**

Approximate increase over baseline:

**+6°C**

The temperature peak followed the utilization increase by approximately two seconds.

Temperature subsequently returned toward approximately 44–45°C.

The supplied telemetry contained **no evidence of thermal throttling**.

This statement is limited strictly to the supplied telemetry and is not a general hardware diagnosis.

---

## 60-Second Window Statistics

For the supplied 60-sample dependency window:

### GPU Utilization

- Minimum: **0%**
- Average: **13.07%**
- Maximum: **100%**

### GPU Temperature

- Minimum: **44°C**
- Average: **45.28°C**
- Maximum: **51°C**

---

## State Transition

Before dependency resolution:

`STATE₀`

contained verified telemetry but insufficient resolution to determine whether the 100% GPU utilization measurement represented an isolated sample or sustained activity.

The model requested:

`Δ_dependency`

containing the approximately one-minute high-resolution telemetry window.

After cross-checking and incorporating that dependency:

`STATE₀ + Δ_dependency → STATE₁`

`STATE₁`

contained sufficient evidence to resolve the question.

The model then reported:

**No additional dependency required.**

This represents explicit dependency closure.

---

## What This POC Establishes

This experiment provides evidence that, in this specific Qwen3.8-27B / LM Studio / Open WebUI configuration:

- A fresh model session can follow the Little Engine protocol without prior Little Engine-specific training or persistent conversation memory.
- The model can operate from externally supplied verified state.
- The model can detect insufficient evidence.
- The model can refuse to fabricate missing information.
- The model can request a narrowly scoped dependency.
- A targeted dependency delta can be supplied without resending the complete source dataset.
- The model can cross-check new evidence against previously verified state.
- The model can preserve valid prior state when the new evidence does not contradict it.
- The model can revise a previous conclusion when higher-resolution evidence warrants revision.
- The model can update an artifact using the new verified evidence.
- The model can explicitly close the dependency when sufficient information has been supplied.

---

## What This POC Does Not Establish

This experiment does **not** establish:

- Universal compatibility across AI models.
- Universal compatibility across interfaces.
- That Little Engine is currently an automated middleware/controller.
- Exact token savings.
- Exact compute savings.
- Exact latency improvements.
- Exact memory savings.
- Exact power savings.
- Exact monetary cost savings.
- That Little Engine is more accurate than conventional retrieval systems.
- That reduced-context analysis is equivalent to full raw-data analysis.
- That Little Engine makes a smaller model equivalent to a larger model.
- Production readiness.
- Commercial viability.
- Patentability.
- Novelty relative to existing context-management, RAG, caching, memory, incremental-computation, or agent architectures.

The controller, dependency retrieval, and state-management steps in this experiment were performed manually.

---

## Architectural Interpretation

The experiment supports continued investigation of Little Engine as an **external verified-state and context-management layer** rather than merely a prompt-shortening or caching mechanism.

The emerging architecture is:

`Source → Verified State → Relevant Context → Model`

When the model encounters insufficient information:

`Model → Dependency Request → Targeted Retrieval → Verification → Dependency Delta → Model`

After successful resolution:

`Previous Verified State + Dependency Delta → Updated Verified State`

The model therefore does not necessarily require the complete underlying environment to remain inside its active context for every step of a task.

Whether this architecture provides meaningful efficiency, accuracy, cost, or scalability advantages requires additional controlled testing.

---

## Manual Controller Limitation

Little Engine was **not operating as automated middleware** during this experiment.

The orchestration loop was performed manually.

The manual controller was responsible for:

1. Maintaining the verified state.
2. Receiving the model's dependency request.
3. Returning to the original source.
4. Extracting the requested telemetry range.
5. Supplying only the requested fields and time window.
6. Explicitly identifying unavailable fields.
7. Returning the dependency delta.
8. Preserving the previous state across the interaction.

A future automated Little Engine implementation would need to perform these operations programmatically and reproducibly.

---

## Future Validation

Future experiments should evaluate:

- Repeated runs using the same model and configuration.
- Different local models.
- Cloud-hosted models.
- Different task types.
- Larger source datasets.
- Software repositories.
- Long-running agent workflows.
- Multiple sequential dependency requests.
- Conflicting dependency data.
- State invalidation.
- State versioning.
- Automated dependency parsing.
- Automated targeted retrieval.
- Provenance and integrity verification.
- Exact token consumption.
- Prompt-processing latency.
- Generation latency.
- GPU utilization.
- VRAM utilization.
- System memory utilization.
- Retrieval volume.
- Failure rate.
- Task accuracy.

Cross-model testing is required before describing the protocol as model-independent or universally portable.

---

## Artifact Preservation

The following artifacts should be preserved with this experiment:

### `raw/`

- Original hardware telemetry CSV.
- Untouched source data used for the experiment.

### `prompts/`

- Original Little Engine task packet.
- Three-point dependency delta.
- 60-sample dependency delta.
- Resume instructions.

### `responses/`

- Initial insufficiency response.
- Three-point analysis response.
- Dependency request.
- Final dependency-resolution response.

### `screenshots/`

- Open WebUI 32,918-token context failure.
- Model dependency request.
- Final visualization.
- Final dependency-closure statement.

### `results/`

- Final generated visualization.
- Extracted 60-sample dependency dataset.
- Any validated experiment summaries.

Original evidence should not be silently modified.

If an artifact must be corrected or superseded, preserve the original and document the replacement.

---

## Evidence Handling Rule

Little Engine experimental records should distinguish between:

**OBSERVED**  
Directly seen in an experiment.

**SUPPORTED**  
A conclusion reasonably supported by multiple observations or measurements.

**HYPOTHESIS**  
A claim requiring additional testing.

**NOT ESTABLISHED**  
A claim for which current evidence is insufficient.

Failed experiments and contradictory results should be preserved alongside successful results.

---

## Current Conclusion

**Manual Closed-Loop POC: PASS**

The direct raw-file retrieval path exceeded the configured context window before the task could run.

Under the Little Engine protocol, the model operated from a limited verified state, identified missing evidence, requested a narrowly scoped dependency, consumed only the returned dependency delta, cross-checked that evidence against prior state, updated its conclusion and visualization, and explicitly closed the dependency.

This result justifies further controlled testing and development of an automated Little Engine controller.

It does not justify broader claims without additional evidence.

---

## Project Principle

> **Proof, not claims.**