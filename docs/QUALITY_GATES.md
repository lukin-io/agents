# Quality Gates

This document explains the verification and audit gates used by the toolkit.

`AGENTS.md` remains the authority for mandatory command order. This file explains why the gates exist, when to use them, and how to report their results.

## Gate Model

Implementation is not complete when code is written.

A task is complete only after:

```text
contract alignment check
  -> bin/verify
  -> bin/contract_audit --all
  -> derived docs / changelog
  -> final report with evidence
```

## Gate 1: Contract Alignment Check

Before running commands, re-check the implementation against the requirement contract.

Confirm:

- every required endpoint exists
- HTTP method and path match
- auth requirements match
- params match names, types, requiredness, defaults, and enums
- response fields are owned by Blueprinter
- required fields cannot serialize as `null`
- documented validations are enforced
- canonical error envelopes are used
- no TODO or placeholder gaps remain

Discrepancies use the required taxonomy:

```text
[IMPL] expected per docs vs actual code
[DOC] docs conflict with model/DB reality
```

Fix all `[IMPL]` discrepancies before continuing.

## Gate 2: `bin/verify`

`bin/verify` is the primary quality gate runner.

Default command:

```bash
bin/verify
```

The default fast profile is intended for normal pre-merge use.

It includes:

- RuboCop
- RSpec, excluding specs tagged `:full_only`
- RSwag Swagger generation when API/spec/doc surfaces changed

Use the full profile only when required by `AGENTS.md` or explicitly requested:

```bash
bin/verify --full
```

The full profile includes:

- RuboCop
- RSpec
- Brakeman
- Bundle Audit
- RSwag Swagger generation
- seed replant

## Gate 3: `bin/contract_audit --all`

Run after `bin/verify` passes:

```bash
bin/contract_audit --all
```

This gate protects the workflow from representational and documentation drift.

It checks:

- requirement docs remain read-only during implementation
- generated Swagger YAML is not hand-edited without rswag spec changes
- database-specific SQL tokens are not introduced
- routes avoid kebab-case and trailing slashes
- controllers do not introduce ad-hoc JSON hashes
- Flow/PRD docs keep the required template structure

## Handoff Exception

A requirement doc may appear in the diff only as upstream handoff input.

The normal allowed case is:

- a new requirement version heading was added before implementation
- no existing requirement version heading was removed
- implementation follows that new requirement version

Do not use requirement allowlists to hide requirement edits made during implementation.

## Reporting Requirements

Final reports must include a `CHECKS` section.

Include the exact command and exit code:

```text
CHECKS
- bin/verify: exit 0
- bin/contract_audit --all: exit 0
```

If a non-standard requirement handoff allowlist is used, include the exact command:

```text
- bin/contract_audit --all --allow-requirement doc/requirements/FEATURE.md: exit 0
```

## Why These Gates Matter

These gates reduce the common failure modes of AI-assisted implementation:

- code generated before the contract was fully read
- undocumented response-shape changes
- generated docs drifting from specs
- controller-level JSON bypassing Blueprinter
- requirement docs being rewritten to match implementation
- docs being updated before verification passes
- final reports lacking evidence

The goal is mergeable-by-default work: small diffs, explicit contract mapping, green checks, and reviewable proof.
