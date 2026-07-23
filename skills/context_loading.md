# Skill: Context Loading

Use this skill to execute normative **Phase -1: Context Loading** from `AGENTS.md`.

This skill is an execution aid. `AGENTS.md` and `AGENTS_CONTRACT.md` remain authoritative.

## Purpose

Load the minimum complete context required for correct planning without flooding the working context with unrelated files.

Minimum complete context includes every normative rule, requirement version, activated procedure, dependency, and implementation surface that can materially affect correctness.

## Activation

Activate this skill before Phase 0 for tasks involving:

- `Execute per AGENTS.md`
- a requirement document
- an API endpoint or response contract
- feature implementation, refactor, or bug fix
- Flow/PRD updates
- contract audit or discrepancy review

Invocation status should begin as:

```text
SKILL INVOCATION
- Skill: skills/context_loading.md
- Phase: Phase -1
- Trigger: repository task requires context loading before planning
- Inputs resolved: YES/NO
- Required output: CONTEXT INVENTORY + CONTEXT SUMMARY
- Status: ACTIVATED/BLOCKED
- Notes:
```

## Required Inputs

- task id or stable task label
- feature label
- requirement path under `doc/requirements/**`
- expected Flow path under `doc/flow/**`
- expected PRD path under `doc/prd/**`

If the requirement path is unknown, locating it is part of Phase -1. Do not infer a contract from derived docs.

## Context Classifications

Classify each source as:

- `ENTRYPOINT` — root loading and authority instructions.
- `NORMATIVE` — mandatory workflow and engineering rules.
- `SOURCE` — canonical requirement contracts.
- `PROCEDURE` — activated skill files.
- `DERIVED` — Flow/PRD/changelog records.
- `EVIDENCE` — code, routes, schema, specs, generated docs, migrations, seeds, and config.
- `EXPLANATORY` — guides, concepts, examples, and migration notes.

This skill must classify itself as `PROCEDURE` in the context inventory.

## Steps

1. Read `AGENTS.md`.
2. Identify the task, feature, and requirement source.
3. Read relevant `AGENTS_CONTRACT.md` sections plus every normative dependency they reference.
4. Read the target requirement document and all requirement-owned versions.
5. Identify the latest requirement-owned version label without inventing a new version.
6. Identify other candidate skills for later phases without bulk-loading their full bodies.
7. Locate mirrored Flow/PRD docs and classify them as `DERIVED`.
8. Scan implementation evidence:
   - routes
   - controllers
   - models
   - blueprints
   - policies
   - services, queries, or jobs
   - request, policy, model, blueprint, and rswag specs
   - migrations, schema, seeds, and relevant configuration
9. Expand context only for discovered dependencies, references, conflicts, or ambiguity.
10. Build the context inventory and summary.
11. Report only relevant excerpts, paths, signatures, and compact behavior summaries.

## Progressive Disclosure

- Do not load every skill or explanatory document by default.
- Load sources that are authoritative, directly referenced, dependency-owning, procedure-owning for the current phase, or needed to resolve ambiguity.
- Requirement documents and all their relevant versions are mandatory.
- Derived docs may help navigation but cannot own behavior.
- Evidence describes current behavior but cannot rewrite the requirement contract.
- Later-phase skills may be identified by path without loading their full body until activation.
- Update the inventory whenever later investigation adds context.

## Expected Output

```text
CONTEXT INVENTORY
| Path/Source | Classification | Why loaded | Status |
| --- | --- | --- | --- |

CONTEXT SUMMARY
- Task / feature:
- Requirement source:
- Requirement versions considered:
- Normative sections loaded:
- Procedures activated:
- Derived docs loaded:
- Implementation surfaces scanned:
- Related specs found:
- Dependencies discovered:
- Context gaps:
- Ready for Phase 0: YES/NO
```

## Completion Check

Before Phase 0, confirm:

- task and requirement source are identified
- relevant normative rules were read
- all requirement-owned versions were considered
- current-phase procedures were identified and activated
- source, procedure, derived, evidence, and explanatory context are distinguished
- implementation and spec surfaces were scanned sufficiently
- no material unresolved gap can change the plan
- no code change was made

On success, update the invocation record:

```text
SKILL INVOCATION
- Skill: skills/context_loading.md
- Phase: Phase -1
- Trigger: repository task requires context loading before planning
- Inputs resolved: YES
- Required output: CONTEXT INVENTORY + CONTEXT SUMMARY
- Status: COMPLETED
- Notes: Ready for Phase 0
```

## Failure Modes

Set the invocation status to `BLOCKED`, output `Ready for Phase 0: NO`, and stop when:

- the requirement source is missing
- requirement version ownership is ambiguous
- a referenced normative file or requirement version cannot be loaded
- multiple primary Flow/PRD docs appear to own the same feature
- a material implementation dependency is unresolved
- docs and implementation disagree in a way that changes behavior
- an authority conflict cannot be resolved

Use `[IMPL]` and `[DOC]` labels where the discrepancy taxonomy applies.
