# Little Engine - Public Release Provenance

## Release Candidate

This repository is a sanitized public derivative of the private Little Engine experimental evidence archive.

Public release candidate:

`LE-PUBLIC-RC1`

Authoritative private source revision:

`22a0e42f2ff0cde7902b5232919c0f392f62e258`

The private archive remains the authoritative record of the original experimental artifacts.

## Publication Sanitization

The public derivative intentionally removes or normalizes identifying environment metadata.

Sanitized fields include:

- local user-home paths
- workstation hostnames
- persistent GPU device UUIDs

Explicit markers such as:

`<REDACTED_USER_HOME>`

`<REDACTED_HOSTNAME>`

`<REDACTED_GPU_UUID>`

identify publication redactions.

These changes are disclosure sanitization and are not experimental transformations.

## Evidence Integrity

Experimental outcomes, recorded model responses, measurements, PASS/FAIL classifications, and experiment IDs are not intentionally altered by the publication sanitization process.

Because publication sanitization changes file bytes, hashes of sanitized public artifacts may differ from hashes recorded in the authoritative private archive.

Public-release artifacts therefore receive a separate public verification manifest.

Historical private manifests are retained only where useful for provenance and must not be interpreted as hashes of sanitized public copies.

## Evidence Boundaries

The public repository documents experimental observations and supported behavior only within the configurations actually tested.

The project does not establish:

- universal model agnosticism
- universal provider portability
- universal operating-system portability
- generalized token savings
- generalized compute or energy savings
- generalized latency or cost savings
- production readiness
- commercial viability
- patent novelty or validity

Historical evidence gaps are disclosed rather than reconstructed.

No missing historical output is recreated or inferred for publication.

## Preservation Policy

The private evidence archive remains frozen.

Corrections, errata, publication notes, and public verification metadata are maintained separately from the original experimental record.
