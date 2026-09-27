# Little Engine - Codex Operating Instructions

## Project State

Little Engine is an experimental AI state/context governance mechanism.

The repository evidence is authoritative. Do not reconstruct project state from assumptions, prior chats, or missing context.

Closed experiments remain frozen and must not be modified.

## Core Protocol

Use three authoritative inputs:

1. VERIFIED STATE
2. RELEVANT DELTA
3. REQUESTED DEPENDENCY DELTA

Rules:

- VERIFIED STATE remains authoritative until explicitly updated by validated evidence.
- Do not guess, infer, reconstruct, or silently fill missing state.
- If required information is missing, STOP.
- Request only the single minimum missing dependency.
- Evaluate incoming information only against the currently requested dependency.
- Unrelated information does not satisfy the dependency and does not advance state.
- If the requested dependency is satisfied, validate and merge only that relevant delta.
- Resume from the existing verified state.
- Produce UPDATED STATE only after dependency sufficiency is established.

Conceptual form:

Context(t+1) =
Verified Context(t)
+ Relevant Change Delta
+ Requested Dependency Delta

## Experiment Governance

Before executing any change, apply a scope gate.

If a requested change could alter:

- the Little Engine core mechanism
- protocol semantics
- claim boundaries
- frozen experiment evidence
- established experiment methodology

STOP and treat that change as a separate proposed experiment.

Do not modify closed evidence to improve results.

PASS and FAIL are both valid completed experimental outcomes.

## Evidence Rules

Formal evidence may include:

- experiment IDs
- frozen prompts / fixtures
- commands or scripts
- raw model/runtime outputs
- environment snapshots
- telemetry
- model/runtime versions
- SHA-256 hashes
- technical screenshots
- documented PASS / FAIL / STOP outcomes
- explicit evidence boundaries

Do not place casual conversation, jokes, mood commentary, personal discussion,
or unrelated chat content into formal evidence.

Never expose credentials, tokens, passwords, API keys, or private secrets.

## Claim Discipline

Distinguish:

### Observed
Directly recorded in experiment evidence.

### Supported
Reasonable conclusion supported by the recorded experiment.

### Not Established / Out of Scope
Anything the experiment did not test.

Do not inflate claims.

Do not claim universal superiority, universal model compatibility, production
readiness, generalized compute/energy/cost savings, or patent validity without
separate evidence.

## Current Frozen Reference

LE-LINUX-CLOSEOUT-001 is CLOSED.

Its evidence must not be altered.

Linux is a frozen reference environment.

New portability, provider, agent, orchestration, Windows, or Codex tests must
use new experiment IDs.

## Codex Work

Codex is a replaceable execution/provider layer.

Little Engine owns the state/governance semantics.

Codex must not become the authoritative state store.

For Codex portability testing:

- use frozen fixtures where applicable
- preserve raw outputs
- record the exact model/configuration used
- do not change fixture wording after testing starts
- classify results strictly against the predefined contract
- preserve failures rather than tuning until PASS
- use a new experiment ID

Current planned experiment:

NONE - no new experiment is authorized during LE-PUBLIC-RC1.

