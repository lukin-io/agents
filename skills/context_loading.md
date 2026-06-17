# Skill: Context Loading

Use this skill before planning or implementation.

## Purpose

Load the minimum complete context needed for a task without flooding the working context with unrelated docs or files.

## When To Use

Use this skill when a task references:

- `Execute per AGENTS.md`
- a requirement document
- a Flow or PRD doc
- an API endpoint
- a feature implementation
- a contract audit or discrepancy review

## Required Inputs

- task id
- feature label
- requirement doc path under `doc/requirements/**`
- expected Flow doc path under `doc/flow/**`
- expected PRD doc path under `doc/prd/**`

## Steps

1. Read relevant normative sections from `AGENTS.md`.
2. Read the target requirement document, including all versions.
3. Identify the latest requirement-owned version label.
4. Locate mirrored Flow/PRD docs if they already exist.
5. Treat Flow/PRD docs as derived context only.
6. Search implementation surfaces:
   - routes
   - controllers
   - models
   - blueprints
   - policies
   - services or queries
   - request specs
   - rswag specs
   - migrations and seeds
7. Build a compact context summary.
8. Report only relevant excerpts, paths, signatures, and short behavior summaries.

## Expected Output

```text
CONTEXT SUMMARY
- Requirement source:
- Latest requirement version:
- Derived Flow doc:
- Derived PRD doc:
- Existing implementation surfaces:
- Known related specs:
- Context gaps:
```

## Verification

Before moving to planning, confirm:

- requirement source was read
- all requirement versions were considered
- derived docs were not treated as source of truth
- implementation surfaces were scanned
- no code changes were made

## Failure Modes

Stop and report when:

- requirement path is missing
- multiple primary Flow/PRD docs appear to own the same feature
- requirement versioning is ambiguous
- implementation exists but no related specs are found
- docs and implementation disagree in a way that changes behavior

Use `[IMPL]` and `[DOC]` labels for discrepancies.
