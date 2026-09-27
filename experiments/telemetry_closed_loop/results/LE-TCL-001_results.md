LE-TCL-001 — Results Record
Experiment: Little Engine Telemetry Closed-Loop Experiment  
Experiment ID: LE-TCL-001  
Date: September 12, 2026  
Result: PASS  
Validation Level: Manual Proof of Concept  
Controller: Manual  
Model: Qwen3.8-27B, GGUF Q4_K_M (~17.74 GB)  
Runtime: LM Studio  
Interface: Open WebUI  
Configured Context Limit: 32,768 tokens  
Test Hardware: AMD Radeon RX 7900 XTX, 24 GB VRAM
---
Executive Result
LE-TCL-001 demonstrated a complete manual Little Engine closed loop on real hardware telemetry.
In the control path, direct retrieval of the larger telemetry source assembled a 32,918-token request against a configured 32,768-token context limit. The request exceeded the available context by approximately 150 tokens and failed before the requested analysis could run.
In the Little Engine path, the model was instead given a compact verified state under explicit insufficiency and non-fabrication rules. When the available state was insufficient to resolve the duration of a 100% GPU utilization sample, the model requested a narrowly scoped dependency: approximately one minute of GPU utilization and GPU temperature telemetry at ~1 Hz.
The manual controller retrieved only that requested dependency from the authoritative source. The model cross-checked overlapping values against prior verified state, revised its earlier conclusion, updated the analysis, and explicitly closed the dependency.
The experiment therefore produced the observed sequence:
```text
CONTROL:
Raw retrieval
→ 32,918-token assembled request
→ 32,768-token configured limit exceeded
→ task did not run

LITTLE ENGINE:
Verified state
→ limited truthful result
→ unresolved question
→ minimum dependency request
→ targeted verified delta
→ cross-check
→ state update
→ revised result
→ dependency closed
```
---
Primary Result
The strongest supported result from LE-TCL-001 is:
> In this specific Qwen/Open WebUI experiment, external context reduction and dependency-on-demand allowed a task to proceed within a fixed 32K context where direct raw-file retrieval exceeded that context. The model respected missing-data/non-fabrication constraints, requested a narrowly scoped dependency, consumed only the returned delta, cross-checked it against prior verified state, updated its prior conclusion and visualization, and closed the dependency without requiring the original full dataset.
This statement is intentionally limited to the observed experiment.
---
Control Path Result
Source Dataset
The source telemetry contained:
601 rows total
1 summary row with blank timestamp
600 timestamped samples
timestamp range: 2026-09-12 16:03:00.315 → 16:12:59.081
approximately 1-second sampling
Relevant source columns included:
GPU utilization
GPU temperature
CPU utilization
system memory utilization
CPU temperature was not present in the source telemetry.
System memory was recorded as utilization percentage, not GB.
Direct Retrieval
A larger hardware telemetry CSV was attached through Open WebUI.
Open WebUI assembled a request of:
32,918 tokens
Configured context:
32,768 tokens
Observed error:
```text
request (32918 tokens) exceeds the available context size (32768 tokens)
```
Control Verdict
FAIL — context packing overflow before task execution.
This result does not establish that Qwen was incapable of analyzing the source data. The failure occurred because the assembled request exceeded the configured context window.
It also does not establish that all conventional retrieval or RAG systems fail under similar conditions.
---
Little Engine Path Result
Stage 1 — Fail-Closed Validation
The initial Little Engine packet contained placeholders rather than populated telemetry.
The model:
recognized insufficient evidence
did not fabricate telemetry
requested missing information
Verdict: PASS.
Stage 2 — Compact Verified State
A three-point co-timestamped verified telemetry state was supplied.
The model:
produced a limited truthful visualization
excluded unavailable CPU temperature
detected the system-memory unit mismatch
rejected unverified placeholder summary statistics
distinguished measured samples from unobserved intervals
identified the unresolved question surrounding the 100% GPU sample
Verdict: PASS.
Stage 3 — Dependency-on-Demand
The model requested the minimum additional evidence needed to resolve the specific uncertainty:
```text
16:12:00 → 16:13:00
approximately 1 Hz
GPU utilization + GPU temperature
CPU temperature if available
```
The model indicated that it did not need the complete raw CSV or the earlier telemetry gap for this specific question.
Verdict: PASS.
Stage 4 — Targeted Retrieval
The manual controller retrieved exactly the requested one-minute source window:
60 timestamped samples
GPU utilization
GPU temperature
CPU temperature unavailable
The complete raw CSV was not resent.
Verdict: PASS.
Stage 5 — State Verification
The returned dependency overlapped two measurements from the previous verified state:
```text
16:12:14.081 | GPU 100% | GPU TEMP 46 C
16:12:59.081 | GPU 5%   | GPU TEMP 44 C
```
Both matched.
No contradiction was detected.
Verdict: PASS.
Stage 6 — Updated Result
The dependency resolved the uncertainty around the 100% sample.
Primary burst:
```text
16:12:14.081 | GPU 100% | TEMP 46 C
16:12:15.083 | GPU 96%  | TEMP 50 C
16:12:16.086 | GPU 88%  | TEMP 51 C
```
Elapsed first-to-last sample time:
2.005 seconds
Supported interpretation:
approximately 2–3 seconds of sustained high GPU utilization at the available sampling rate.
The one-minute window also showed additional brief high-utilization activity, including:
```text
16:12:01.083 | GPU 69% | TEMP 47 C
16:12:02.081 | GPU 53% | TEMP 49 C
16:12:09.083 | GPU 94% | TEMP 50 C
```
Verdict: PASS.
Stage 7 — Dependency Closure
The model explicitly reported:
> **No further dependency needed to answer the requested resolution.**
Verdict: PASS.
---
Resolved Telemetry Findings
For the requested 60-sample dependency window:
GPU Utilization
Minimum: 0%
Average: 13.07%
Maximum: 100%
GPU Temperature
Minimum: 44 C
Average: 45.28 C
Maximum: 51 C
Primary Burst
100% → 96% → 88%
first-to-last measured duration: 2.005 seconds
supported duration interpretation: approximately 2–3 seconds
Thermal Response
approximate baseline: 45 C
burst-start temperature: 46 C
peak: 51 C
approximate increase: +6 C
observed peak followed the utilization rise by approximately 2 seconds
The supplied telemetry showed no evidence of thermal throttling within the supplied data. This is not an absolute hardware diagnosis.
---
State Transition Result
The observed state progression was:
```text
STATE_0
    +
Δ_dependency
    =
STATE_1
```
Operationally:
```text
Verified State
→ Task
→ Uncertainty
→ Request Minimum Dependency
→ Retrieve Targeted Source Delta
→ Verify Against Existing State
→ No Contradiction
→ Merge Relevant Delta
→ Update Conclusion
→ Close Dependency
```
This is the key behavioral result of LE-TCL-001.
---
What LE-TCL-001 Establishes
The experiment establishes that, in this specific manual test:
Direct raw-file retrieval exceeded the configured context window.
A compact verified-state path fit within the available context.
The model obeyed explicit missing-data and non-fabrication constraints.
The model could operate from deliberately incomplete but verified state.
The model identified a specific unresolved question.
The model requested a narrowly scoped dependency rather than the complete source.
The requested source delta was sufficient to resolve that question.
The model cross-checked new evidence against prior verified state.
The model revised its prior conclusion when the new evidence materially changed it.
The model explicitly closed the dependency when no further information was required.
---
What LE-TCL-001 Does Not Establish
This experiment does not establish:
universal model compatibility
universal interface compatibility
universal superiority over RAG or conventional retrieval
automated Little Engine middleware
production readiness
exact token savings percentage
exact compute savings
exact latency savings
exact power savings
exact monetary cost savings
accuracy superiority
equivalence between the compact state and the complete 600-point dataset for every possible question
commercial viability
novelty
patentability
These remain unproven or require additional controlled testing.
---
Architectural Interpretation
LE-TCL-001 suggests that Little Engine should not be described primarily as a caching mechanism.
Caching, hashing, fingerprints, and deltas can be implementation mechanisms underneath the system.
The larger architectural behavior under investigation is:
external verified-state/context management with dependency-on-demand.
A compact representation is:
```text
Context(t+1)
=
Verified Context(t)
+
Relevant Change Delta
+
Requested Dependency Delta
```
And the observed protocol is:
```text
STATE_n
+
Δ
→ task
→ missing Δ?
→ Δ_dependency
→ result
→ STATE_n+1
```
The significance is not simply that the model received less information.
The system attempted to provide the minimum sufficient verified information for the current task, with an explicit path for the model to request additional evidence when necessary.
---
Manual Controller Limitation
Little Engine was not running as automated middleware during LE-TCL-001.
The controller functions were performed manually.
Those functions included:
maintaining the verified-state boundary
interpreting the dependency request
returning to the authoritative source
extracting the requested window and fields
supplying the dependency delta
preserving the evidence chain
Therefore the experiment validates the manual closed-loop behavior, not an automated production implementation.
Automating these controller responsibilities is a future engineering task.
---
Evidence Chain
The frozen evidence package for LE-TCL-001 consists of:
```text
README.md
raw/
prompts/
responses/
screenshots/
results/
```
The screenshot sequence preserves the visual story:
```text
CONTROL FAIL
→ VERIFIED STATE
→ REQUEST Δ
→ RESOLVE Δ
→ CLOSE
```
The prompt record preserves the supplied context and dependency delta.
The response record preserves the model-side behavioral sequence.
The raw folder preserves the authoritative telemetry source.
This results file summarizes the experiment outcome and its evidence boundary.
---
Evidence Handling Rule
The original telemetry and preserved screenshots remain authoritative.
If a later transcription, summary, or interpretation conflicts with original evidence:
preserve the historical artifact
document the discrepancy
defer to the original source evidence
create a corrected version
do not silently rewrite the historical record
---
Final Classification
Experiment: LE-TCL-001  
Result: PASS  
Validation Level: Manual Proof of Concept  
Closed Loop Demonstrated: Yes  
Automated Middleware Demonstrated: No  
Dependency-on-Demand Demonstrated: Yes  
Verified-State Carry-Forward Demonstrated: Yes  
Dependency Closure Demonstrated: Yes  
Universal Claims Established: No
---
Conclusion
LE-TCL-001 demonstrated a complete manual Little Engine state/dependency loop under a fixed 32,768-token context constraint.
The control path exceeded that configured context before task execution. The Little Engine path instead began from compact verified state, allowed the model to identify precisely what evidence was missing, supplied only the requested source dependency, verified that dependency against prior state, updated the result, and closed the dependency without resending the complete source dataset.
The experiment does not prove a production system or universal efficiency advantage.
It does provide concrete evidence that the underlying Little Engine behavior is technically testable and that the manual closed-loop protocol worked in this specific experiment.
> **Little Engine isn't about giving AI less information. It's about giving AI the right information at the right time.**
Proof, not claims.