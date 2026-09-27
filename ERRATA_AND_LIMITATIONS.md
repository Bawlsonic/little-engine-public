# Little Engine - Errata and Evidence Limitations

This document records known discrepancies, provenance limitations, and interpretation boundaries identified during preparation of LE-PUBLIC-RC1.

The authoritative private experimental archive is not rewritten to correct historical records. Public explanations and errata are maintained separately.

## 1. LE-TCL-001 Temperature Erratum

A review of the preserved telemetry identified a temperature transcription and arithmetic discrepancy.

For the reviewed 60-sample window:

- source telemetry average: 45.35 C
- transcribed prompt average: approximately 45.333 C
- historically reported average: 45.28 C

At timestamp 16:12:26.086:

- source CSV: 46 C
- transcribed prompt: 45 C

The documented GPU-utilization average of 13.07 percent reproduces.

This temperature discrepancy does not by itself change the recorded behavioral result, but the historical 45.28 C value should not be treated as reproduced from the preserved source telemetry.

## 2. Linux / Windows Fixture Representation

Linux and Windows A/B/C fixtures require representation-specific language.

In the publication checkout, line-ending representation may differ.

After line-ending normalization, corresponding fixture text is identical.

The committed private HEAD blobs inspected during the RC1 audit were byte-identical LF for the corresponding fixture pairs.

The exact transported byte representation used during every original execution cannot be reconstructed solely from the later checkout.

Accordingly, public descriptions should distinguish text equivalence from byte representation rather than making an unqualified byte-identical claim.

## 3. Historical Manifest Limitations

Some historical Linux, Windows, and Codex manifests were produced using PowerShell table output.

Known limitations include:

- absolute local paths
- truncated displayed paths
- line-ending-dependent verification behavior
- incomplete inclusion of some later closeout files

These historical manifests are retained as evidence of the original record.

They are not the verification mechanism for sanitized LE-PUBLIC-RC1 artifacts.

The public release receives a separate verification manifest.

## 4. Codex Portability Evidence

LE-CODEX-PORTABILITY-001 reports PASS results across the tested Codex configurations.

However, the preserved experiment package does not contain the complete actual run responses for all reported runs.

The package primarily preserves expected contracts and narrative result documentation.

Therefore these outcomes should be described as reported experiment results with incomplete raw-output provenance.

Missing historical responses are not recreated for public release.

## 5. Claude Code Portability Evidence

LE-CLAUDE-CODE-PORTABILITY-001 preserves annotated JSON experiment records containing the recorded response strings and classifications.

Claude Code desktop did not expose a native per-turn API response envelope to the experiment session.

Therefore:

- per-turn token telemetry was not captured
- per-turn latency telemetry was not captured
- the bundled desktop runtime version was not captured
- the JSON records should not be described as native provider API envelopes

The experiment records support comparison of the recorded behavioral contract within the tested configuration.

They do not establish performance or efficiency comparisons with Ollama or Codex.

## 6. PASS Whitespace Ambiguity

At least one preserved GPT-OSS response contains trailing whitespace while its fields otherwise conform to the expected response contract.

The historical PASS classification is preserved.

The original methodology did not define a sufficiently precise whitespace-normalization policy for interpreting the word "strict."

Public interpretation should therefore treat this as field/response conformance with a documented whitespace ambiguity rather than claiming proven byte-for-byte response equality.

## 7. Repeatability Language

Some historical documentation uses the word "repeatable" when only Run 01 exists for a particular model/phase configuration.

Multiple phases or multiple configurations do not establish within-configuration run-to-run repeatability.

Public documentation therefore distinguishes:

- reproduction across tested configurations
- replication across environments
- repeat runs of the same configuration

Run-to-run variance is not established where repeated runs were not performed.

## 8. Reproducibility Limitations

The historical evidence does not preserve every detail required for exact execution reconstruction.

Depending on the experiment, missing information may include:

- exact API request bodies
- complete runner commands
- model-template handling
- session-isolation procedure
- exact comparator implementation
- runtime/build information
- complete original session transcripts

Approximate reproduction may be possible for some experiments.

Exact historical invocation reconstruction is not claimed.

## 9. Claim Boundary

The preserved evidence supports investigation of Little Engine's dependency-gated behavior across the specifically tested configurations.

It does not establish:

- universal model agnosticism
- universal provider portability
- universal operating-system portability
- universal frontier-model portability
- persistent autonomous state-store mutation
- autonomous dependency discovery
- actual deployment execution from A/B/C conformance
- generalized token reduction
- generalized compute or energy savings
- generalized latency or cost savings
- production readiness
- commercial viability
- patent novelty or validity

These limitations are part of the evidence record, not exceptions to it.
