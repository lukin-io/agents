# Skill: Context Loading

Use this skill to execute normative **Phase -1: Context Loading** before planning.

This skill is an execution aid. `AGENTS.md`, `AGENTS_CONTRACT.md`, and the target requirements remain authoritative.

## Purpose

Load, classify, and validate the minimum complete context required for correct planning without flooding active context with unrelated files.

The skill produces the `CONTEXT INVENTORY`, `CONTEXT SUMMARY`, and Phase 0 readiness decision.

## Activation

Activate this skill when repository work requires understanding requirements, implementation, verification evidence, or derived docs before planning.

Typical triggers:

- `Execute per AGENTS.md`
- requirement-driven API work
- feature implementation, refactor, or bug fix
- contract discrepancy review
- Flow/PRD work requiring source and evidence discovery

Do not activate it as a substitute for Phase 0 contract extraction or implementation.

Initial invocation record:

```text
SKILL INVOCATION
- Skill: skills/context_loading.md
- Phase: Phase -1
- Trigger:
- Inputs resolved: YES/NO
- Required output: CONTEXT INVENTORY + CONTEXT SUMMARY
- Status: ACTIVATED/BLOCKED
- Notes:
```

## Required Inputs

- task id or stable task label
- feature label or target concern
- requirement path under `doc/requirements/**`, or enough information to locate it
- expected Flow/PRD paths when applicable
- repository access sufficient to inspect relevant evidence

If the requirement path is unknown, locating it is part of the procedure. Do not infer behavior from derived docs.

## Normative References

Load and follow:

- `AGENTS.md`:
  - Mandatory Load Order
  - Authority and Precedence
  - Phase -1 Context Loading
  - Skill Invocation Contract
- `AGENTS_CONTRACT.md` sections relevant to the target task and every normative dependency they reference
- target `doc/requirements/**` sources and all applicable versions

Use `docs/**` only as explanatory context.

## Procedure

### Step 1 — Identify Task and Authority

Identify:

- task and feature/concern
- target requirement source
- expected derived-doc paths
- current phase and likely later skills

Load this skill as `PROCEDURE` context.

### Step 2 — Load Normative Context

Load:

- root `AGENTS.md`
- relevant `AGENTS_CONTRACT.md` sections
- every referenced normative dependency that can affect correctness

Do not treat a partial contract excerpt as complete when it references another mandatory section.

### Step 3 — Load Source-of-Truth Requirements

Read the target requirement and all applicable requirement-owned versions.

Identify:

- latest requirement-owned version label, if any
- cumulative behavior
- explicit removals or breaking changes
- unresolved requirement ambiguity

Do not invent a version or allow implementation evidence to redefine requirements.

### Step 4 — Locate Derived Context

Locate mirrored Flow/PRD and related changelog docs.

Classify them as `DERIVED`. Use them for navigation and verified history only.

### Step 5 — Scan Implementation Evidence

Inspect relevant:

- routes
- controllers
- models
- blueprints
- policies
- services, queries, and jobs
- request, policy, model, blueprint, and rswag specs
- migrations, schema, seeds, generated API docs, and configuration

Classify these sources as `EVIDENCE`.

### Step 6 — Expand by Dependency

Load more context only when a discovered reference, dependency, conflict, or ambiguity requires it.

Later-phase skills may be identified by path without loading their full bodies until activation.

Update the inventory whenever context expands and re-evaluate affected conclusions.

### Step 7 — Classify Context

Use exactly one primary classification per source:

- `ENTRYPOINT`
- `NORMATIVE`
- `SOURCE`
- `PROCEDURE`
- `DERIVED`
- `EVIDENCE`
- `EXPLANATORY`

### Step 8 — Evaluate Gaps and Readiness

Record unresolved context gaps.

A gap is material when it can change:

- contract interpretation
- implementation plan
- authorization or response behavior
- test scope
- verification profile
- documentation ownership

Material gaps block Phase 0.

## Required Output

```text
SKILL INVOCATION
- Skill: skills/context_loading.md
- Phase: Phase -1
- Trigger:
- Inputs resolved: YES/NO
- Required output: CONTEXT INVENTORY + CONTEXT SUMMARY
- Status: COMPLETED/BLOCKED
- Notes:

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

Targeted excerpts must respect `AGENTS_CONTRACT.md` limits.

## Completion Check

The skill is `COMPLETED` only when:

- task and requirement source are identified
- relevant normative rules and dependencies were loaded
- all applicable requirement versions were considered
- context sources are correctly classified
- implementation and spec evidence was scanned sufficiently for planning
- current-phase procedures are activated or explicitly not required/unavailable
- no material unresolved gap can change the plan
- no code change was made
- `Ready for Phase 0: YES`

## Failure Modes

Set the invocation to `BLOCKED`, output `Ready for Phase 0: NO`, and stop when:

- requirement source or version authority is missing/ambiguous
- a referenced normative source cannot be loaded
- multiple primary derived docs create unresolved ownership
- a material dependency is unresolved
- an authority conflict cannot be resolved
- evidence and requirements disagree in a way that makes planning unsafe

Use `[IMPL]` and `[DOC]` labels where the discrepancy taxonomy applies.

## Handoff

On `COMPLETED`:

- next skill: `skills/rails_api_feature.md` when Rails API implementation is required
- next phase otherwise: applicable Phase 0 or direct review procedure under the contract
- artifact carried forward: `CONTEXT INVENTORY` and `CONTEXT SUMMARY`

On `BLOCKED`:

- report the missing source, authority conflict, or material dependency
- resolve it before Phase 0 or any code change
