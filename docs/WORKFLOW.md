# Agentic Rails API Workflow

This document gives a compact map of the workflow defined by `AGENTS.md`.

`AGENTS.md` remains the authoritative contract. This file is an adoption guide for teams that want to understand the execution loop quickly.

## Workflow Summary

```text
1. Requirement handoff
2. Context extraction
3. Repo scan
4. Implementation plan
5. Confirmation gate
6. Implementation
7. Contract alignment check
8. Verification
9. Contract audit
10. Derived docs and changelog
11. Final report
```

## 1. Requirement Handoff

Feature behavior starts in `doc/requirements/**`.

The requirement document is read-only during implementation work.

It owns:

- API behavior
- request parameters
- response shape
- validation rules
- auth rules
- version labels
- compatibility deltas

If a new requirement version was authored upstream before implementation begins, it may be treated as handoff input. Implementation still must not rewrite the requirement while coding.

## 2. Context Extraction

Before code changes, extract the backend contract from requirements.

Required output:

- endpoints
- params
- response fields
- validations
- error codes
- version deltas
- Contract Traceability Matrix

This phase turns prose requirements into implementation-ready context.

## 3. Repo Scan

Scan existing implementation surfaces before planning changes.

Look for:

- routes
- controllers
- models
- blueprints
- policies
- services/queries
- request specs
- rswag specs
- migrations
- seeds
- existing Flow/PRD docs

The goal is to reuse existing Rails surfaces and avoid duplicate implementation paths.

## 4. Implementation Plan

Produce a file-by-file plan before editing code.

Include:

- required components
- changed files
- new files
- deleted files, if any
- test plan
- auth considerations
- preload/N+1 considerations
- safe defaults
- requirement mapping
- risks and discrepancies

Discrepancies must use the required taxonomy:

```text
[IMPL] expected per docs vs actual code
[DOC] docs conflict with model/DB reality
```

## 5. Confirmation Gate

If the task invokes planning-first mode, implementation pauses after the plan.

The expected stop output is:

```text
CONFIRM_TO_IMPLEMENT? (yes/no)
```

No code changes happen before this gate is cleared.

## 6. Implementation

After confirmation, implement with minimal diffs and Rails-way/KISS.

Default implementation rules:

- use Rails primitives first
- add service/query objects only when justified
- keep Blueprinter as the owner of `data` payloads
- keep controllers responsible for envelopes only
- use Pundit for authorization
- use Ransack for filtering/search
- use Kaminari for pagination
- avoid DB-specific SQL unless explicitly allowed by the project

## 7. Contract Alignment Check

Before running verification commands, re-check implementation against requirements.

Confirm:

- endpoint exists
- method/path match
- params match
- response fields exist in blueprints
- required fields are not null
- auth rules match
- validations match
- no TODO/placeholder gaps remain

Fix all `[IMPL]` discrepancies before moving forward.

## 8. Verification

Run the required verification profile:

```bash
bin/verify
```

Use the full profile only when required by `AGENTS.md` or explicitly requested:

```bash
bin/verify --full
```

The fast profile is intended for normal pre-merge use. The full profile is for schema/seed/process-sensitive changes or explicit full verification.

## 9. Contract Audit

Run the contract audit after verification:

```bash
bin/contract_audit --all
```

This checks for drift across critical surfaces, including requirements, Swagger, controller rendering, route style, database-specific SQL tokens, and docs templates.

## 10. Derived Docs and Changelog

Only after verification and contract audit pass:

- update `doc/flow/**`
- update `doc/prd/**`
- add `changelogs/unreleased/{TASK_ID}.md`

Flow and PRD docs are derived context. They must not invent independent API versions.

## 11. Final Report

Final output must include:

- `WHAT & HOW`
- `RATIONALE`
- `CHECKS`
- `RULE COMPLIANCE AUDIT`
- `Discrepancies Report`
- `Contract Traceability Matrix`

This makes the result reviewable by humans and reusable as future context for agents.

## Human / Agent Responsibility Split

The workflow is designed for both human engineers and coding agents.

Humans own:

- product intent
- requirement approval
- architectural tradeoffs
- confirmation gates
- merge decisions

Agents can execute:

- contract extraction
- repo scanning
- implementation planning
- code changes
- verification runs
- contract audit reports
- Flow/PRD/changelog updates

The system works best when requirements are explicit and verification is mandatory.

## Why This Workflow Exists

The workflow reduces the common failure modes of AI-assisted development:

- coding before understanding the contract
- missing hidden auth or response-shape rules
- producing inconsistent JSON envelopes
- drifting from generated API docs
- updating docs before tests pass
- losing traceability between requirements, implementation, and specs

The result should be mergeable-by-default work: small diffs, explicit contract mapping, green checks, and reviewable evidence.
