# LE-CLAUDE-CODE-PORTABILITY-001 — Environment / Baseline Record

## Status

OPEN — baseline recorded. No phase executed.

## Record Scope

This file records only the pre-test environment and baseline state for
LE-CLAUDE-CODE-PORTABILITY-001.

It does not contain results, methodology changes, or claims.

Phase A has not been executed.

## Activation Gate

A read-only repository review was performed before this record was created.

Observed:
- No repository files were modified during the activation check.
- LE-LINUX-CLOSEOUT-001 identified as CLOSED / frozen.
- LE-WINDOWS-PORTABILITY-001 identified as CLOSED / frozen.
- LE-CODEX-PORTABILITY-001 identified as CLOSED / frozen.
- Completed experiments distinguished from planned work.
- LE-CLAUDE-CODE-PORTABILITY-001 identified as the new experiment ID.
- Evidence boundaries preserved.
- Two documentation inconsistencies were reported and deliberately left
  uncorrected under instruction (see Deferred Items).

Activation result: PASS

## Claude Code Environment

- execution surface: Claude Code desktop
- selected model: Opus 5
- standalone CLI version: unavailable / not installed on PATH
- desktop bundled runtime version: NOT YET CAPTURED
- repository baseline: 8825caa91a4d196a2e28ece5a46a3be60dc08925
- pre-test working tree: clean

## Host Environment

- operating system: Microsoft Windows [Version 10.0.26200.9457]
- baseline record timestamp (UTC): 2026-09-27T18:14:26Z

## Authoritative Fixture Source

authoritative_fixture_source = experiments/windows_portability/

The frozen Phase A/B/C fixture text in that directory is authoritative for this
experiment. The source fixtures are not copied, modified, or re-worded.

SHA-256 (as read at baseline):

phase_a_prompt.txt
b63cb06486649ef4e2042fecf112e1c90de34bbdf293c5619bdc67f7c170e86a
889 bytes

phase_b_irrelevant_prompt.txt
48da02763dcb9ba8aafa8efd077be7e3ebea386aaf75db54cd4adae2a337111d
716 bytes

phase_c_valid_dependency_prompt.txt
8122eb721b9344f862efbb491a383c3766639df884dbe0841bec693721a998d6
824 bytes

## Deferred Items — Not Part Of This Experiment

The following were observed during the activation check and are explicitly
excluded from LE-CLAUDE-CODE-PORTABILITY-001 by instruction:

1. AGENTS.md closing line still names LE-CODEX-PORTABILITY-001 as the current
   planned experiment, which is CLOSED. Not reconciled.
2. CURRENT_STATUS.md describes the LE-WINDOWS-PORTABILITY-001 fixtures as
   "byte-identical". The linux_closeout and windows_portability fixture pairs
   are text-identical after line-ending normalization but differ in raw bytes
   (CRLF vs LF). Not reconciled.

Neither item is modified by this experiment. Frozen evidence is untouched.

## Outstanding

- desktop bundled runtime version: to be captured before or alongside Phase A.

## Next Gate

Phase A execution is NOT authorized by this record.

STOP.
