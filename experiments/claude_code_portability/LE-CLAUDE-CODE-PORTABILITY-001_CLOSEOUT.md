# LE-CLAUDE-CODE-PORTABILITY-001 — CLOSEOUT

## Disposition

LE-CLAUDE-CODE-PORTABILITY-001 is CLOSED.

## Summary

The tested Little Engine dependency-gated A/B/C behavior reproduced on a Claude
Code execution surface using the frozen Phase A/B/C fixtures from the CLOSED
LE-WINDOWS-PORTABILITY-001 set.

Activation gate: PASS
Phase A: PASS
Phase B: PASS
Phase C: PASS

First-attempt result: 3 / 3 PASS

## Tested Configuration

- execution surface: Claude Code desktop
- selected model: Opus 5
- effort: High
- activation: PASS
- Phase A: PASS
- Phase B: PASS
- Phase C: PASS
- first-attempt result: 3 / 3 PASS
- authoritative fixture source: experiments/windows_portability/
- standalone CLI version: unavailable / not installed on PATH
- desktop bundled runtime version: NOT CAPTURED
- per-turn token/latency telemetry: NOT CAPTURED
- operating system: Microsoft Windows [Version 10.0.26200.9457]
- repository baseline: 8825caa91a4d196a2e28ece5a46a3be60dc08925
- pre-test working tree: clean

## Observed

Directly recorded in this experiment's evidence:

- A read-only activation review modified no repository files.
- Closed experiments were identified as frozen and distinguished from planned
  work.
- Execution stopped on a single minimum missing dependency before any
  experiment work began.
- Phase A returned the frozen STOP contract exactly.
- Phase B rejected the ui_theme=dark delta, held the dependency OPEN, did not
  advance VERIFIED STATE, and re-requested only the still-missing dependency.
- Phase C merged only the satisfying delta, CLOSED the dependency, updated the
  target to sandbox-b, and RESUMED the requested task.
- In Phase C the previously rejected ui_theme delta was not retroactively
  merged.
- The three source fixtures were unchanged before and after every phase.
- No tracked repository file was modified at any point during the experiment.

## Supported

Within this tested Claude Code configuration, the Little Engine
dependency-gated workflow reproduced the sequence:

VERIFIED STATE
-> missing dependency
-> STOP
-> irrelevant delta rejected
-> dependency remains OPEN
-> requested dependency supplied
-> dependency CLOSED
-> execution RESUMED

This supports behavioral portability of the tested protocol behavior to a
Claude Code agent/provider layer within the defined scope below.

## Evidence Boundary

This experiment establishes behavioral portability only within the single
tested Claude Code configuration recorded above.

This experiment does not establish:

- universal Claude model portability
- universal provider portability
- universal model compatibility
- universal operating-system portability
- token efficiency or exact token savings
- latency efficiency
- compute, energy, or cost efficiency
- production readiness
- commercial viability
- patent novelty or validity

## Scope Limitations

Recorded explicitly rather than implied:

1. One configuration was tested (Claude Code desktop / Opus 5 / High). This is a
   narrower configuration set than LE-CODEX-PORTABILITY-001, which tested three
   model tiers. Additional Claude models, effort levels, or execution surfaces
   require separate experiment records.

2. The desktop bundled runtime version is NOT CAPTURED. The standalone CLI is
   not installed on PATH on the tested host, and the session exposes no runtime
   version string. This experiment therefore cannot be pinned to an exact
   Claude Code build number.

3. Per-turn token and latency telemetry is NOT CAPTURED and was not estimated.
   No efficiency comparison against the Ollama or Codex experiments is
   supported by this evidence.

4. Run 01 only. No repeat runs were performed, so within-configuration
   run-to-run variance is not characterized.

## Relationship To Prior Experiments

- LE-LINUX-CLOSEOUT-001 — CLOSED, frozen reference. Not modified.
- LE-WINDOWS-PORTABILITY-001 — CLOSED. Its fixtures were used as read-only
  authoritative input. Not modified.
- LE-CODEX-PORTABILITY-001 — CLOSED. Not modified.

This experiment does not reopen, alter, or reinterpret any closed experiment.

## Deferred Documentation Items

Two pre-existing documentation inconsistencies were observed during the
activation check and were deliberately NOT reconciled as part of this
experiment, under instruction:

1. AGENTS.md still names LE-CODEX-PORTABILITY-001 as the current planned
   experiment, which is CLOSED.
2. CURRENT_STATUS.md describes the LE-WINDOWS-PORTABILITY-001 fixtures as
   "byte-identical"; they are text-identical after line-ending normalization
   but differ in raw bytes (CRLF vs LF).

Both remain open documentation items outside this experiment's scope.

## Final Disposition

LE-CLAUDE-CODE-PORTABILITY-001 is CLOSED.

Evidence in this directory is frozen and must not be altered.

Future Claude models, effort levels, execution surfaces, or provider
integrations require separate experiment records and do not reopen this closed
experiment.
