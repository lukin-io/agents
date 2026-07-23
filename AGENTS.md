<!--
Author: Maksim Lukin <max@lukin.io>
Copyright (c) 2026 Maksim Lukin
SPDX-License-Identifier: MIT
-->

# AGENTS.md — Agent Operating Contract

This file is the repository entrypoint and authority harness for human engineers and coding agents.

The complete normative Rails API contract is preserved in:

- `AGENTS_CONTRACT.md`

## Mandatory Load Order

Before planning, editing code, running verification, or updating derived documentation:

1. Read this file.
2. Complete **Phase -1: Context Loading** below.
3. Read `AGENTS_CONTRACT.md` in full, or all sections relevant to the task plus every section they reference.
4. Read the target `doc/requirements/**` documents and all requirement-owned versions.
5. Activate and load only the skills relevant to the current phase under the **Skill Invocation Contract** below.
6. Load explanatory docs only when they resolve a current question, dependency, or ambiguity.

Do not make code changes before Phase -1 is complete and the normative contract and target requirements have been loaded.

## Authority and Precedence

1. Explicit user instructions govern the requested outcome unless they conflict with a higher-level safety rule.
2. This file defines the repository-level operating, context-loading, and skill-invocation contract.
3. `AGENTS_CONTRACT.md` defines the complete normative process, engineering rules, verification gates, documentation rules, and final reporting contract.
4. `doc/requirements/**` owns feature/API behavior and numbered requirement versions.
5. `doc/flow/**` and `doc/prd/**` are derived context updated only after verification and compliance gates pass.
6. `skills/**` contains reusable procedures for applying the normative contract; skills cannot override it.
7. `docs/**` is explanatory and adoption-oriented; it is not a second policy authority.

If contract layers, requirements, or a skill appear to conflict, stop and report the conflict. Do not silently choose the less strict or more convenient rule.

## Phase -1 [NORMATIVE] — Context Loading

Phase -1 runs before Phase 0 in `AGENTS_CONTRACT.md`.

### Goal

Load the minimum **complete** context needed for correct planning while avoiding unrelated bulk context.

Minimum context does not mean partial authority. Every rule, requirement version, procedure, dependency, and implementation surface that can affect correctness must be included.

### Context classifications

Classify each loaded source as exactly one primary type:

- `ENTRYPOINT` — this file and mandatory loading/boundary instructions.
- `NORMATIVE` — `AGENTS_CONTRACT.md` sections and other mandatory process rules.
- `SOURCE` — canonical `doc/requirements/**` feature/API contracts.
- `PROCEDURE` — activated `skills/**` files used to execute a phase.
- `DERIVED` — Flow/PRD/changelog docs that describe verified implementation but do not own behavior.
- `EVIDENCE` — routes, code, schema, specs, generated API docs, migrations, seeds, and configuration.
- `EXPLANATORY` — concepts, workflow guides, migration notes, examples, or other non-authoritative aids.

### Required loading sequence

1. Load this entrypoint.
2. Identify the task, feature label, and requirement path.
3. Load relevant `AGENTS_CONTRACT.md` sections and every normative section they reference.
4. Load the target requirement document and all requirement-owned versions.
5. Identify candidate skills by current phase and task type; load only activated skills and classify them as `PROCEDURE`.
6. Locate mirrored Flow/PRD docs and classify them as `DERIVED`.
7. Scan existing implementation evidence:
   - routes
   - controllers
   - models
   - blueprints
   - policies
   - services/queries/jobs when present
   - request, policy, model, blueprint, and rswag specs
   - migrations, schema, seeds, and relevant configuration
8. Expand context only when a discovered dependency, conflict, reference, or ambiguity requires it.
9. Record missing or unresolved context as a gap before planning.

### Progressive disclosure rules

- Do not load every skill, doc, or repository file by default.
- Load a source when it is authoritative for the task, directly referenced, owns a discovered dependency, implements an activated procedure, or is required to resolve ambiguity.
- Requirement documents are never optional when implementing their feature.
- All requirement-owned versions must be considered unless the requirement explicitly states that an older behavior was removed.
- Skills are loaded phase-by-phase; do not keep unrelated skill bodies in active context.
- Derived and explanatory docs may assist understanding but cannot override normative or source-of-truth context.
- Evidence describes current implementation; it does not redefine the requirement contract.
- If additional context is loaded later, update the inventory and re-evaluate affected conclusions.

### Required output before Phase 0

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

Targeted excerpts must follow the excerpt limits from `AGENTS_CONTRACT.md`.

### Phase -1 completion gate

Phase -1 passes only when:

- the task and target requirement source are identified
- relevant normative rules are loaded
- all requirement-owned versions are considered
- relevant procedures are identified and activated, or explicitly marked not required/unavailable
- source, procedure, derived, evidence, and explanatory context are distinguished
- implementation/spec surfaces are scanned sufficiently for planning
- no unresolved context gap can materially change the plan
- no code change has been made

If a material gap remains, output `Ready for Phase 0: NO`, report the gap, and stop before planning.

## Skill Invocation Contract [NORMATIVE]

### Skill role

A skill is a reusable procedure for applying the contract during a specific phase. A skill is not an independent policy source and does not own feature behavior.

### Activation rules

Activate a skill when all are true:

1. The current task and phase match the skill's stated purpose or trigger.
2. Its required inputs are available or can be resolved without violating a stop gate.
3. Its output is required or materially improves repeatability, traceability, or verification.
4. No higher-authority rule forbids its use.

Do not activate a skill merely because it exists.

Default phase mapping for current skills:

- Phase -1: `skills/context_loading.md`.
- Phase 0 through implementation for Rails API feature work: `skills/rails_api_feature.md`.
- Verification/review: `skills/quality_gate_review.md` when present; otherwise follow the normative gates directly.
- Flow/PRD updates after green gates: `skills/flow_prd_update.md`.

### Required invocation lifecycle

For each activated skill:

1. Resolve its required inputs.
2. Load the skill and classify it as `PROCEDURE` in the context inventory.
3. Confirm it does not conflict with this file, `AGENTS_CONTRACT.md`, or requirements.
4. Execute only the phase-relevant procedure.
5. Produce the skill's required artifact/output.
6. Evaluate its completion check and failure modes.
7. Carry forward the resulting artifact and decisions, not unnecessary copies of the full skill body.

### Required invocation record

```text
SKILL INVOCATION
- Skill:
- Phase:
- Trigger:
- Inputs resolved:
- Required output:
- Status: ACTIVATED/COMPLETED/BLOCKED/NOT_REQUIRED/UNAVAILABLE
- Notes:
```

`NOT_REQUIRED` and `UNAVAILABLE` are valid only when the normative workflow can still be executed directly. A missing skill never waives a contract requirement.

### Precedence and conflict rules

- Safety and explicit user constraints remain highest.
- This file and `AGENTS_CONTRACT.md` own process and engineering rules.
- Requirements own feature/API behavior.
- Skills implement procedures within those boundaries.
- Derived and explanatory docs cannot resolve a normative conflict.
- If a skill conflicts with a higher authority, mark it `BLOCKED`, report the conflict, and stop that procedure.
- Do not silently edit a skill during a feature task to make the conflict disappear.

### Skill loading boundaries

- Load only skills needed for the current task and phase.
- Do not bulk-load the skill registry into every phase.
- Multiple skills may be active only when their responsibilities are distinct and their outputs compose without conflicting authority.
- Prefer a single applicable skill over overlapping skills that duplicate ownership.
- A skill may reference normative sections instead of restating them; the referenced sections must be loaded.

### Fallback behavior

When a relevant skill is missing or unavailable:

- follow the normative contract directly
- produce the same required workflow artifacts and evidence
- record the skill as `UNAVAILABLE`
- do not skip or weaken the phase

## Architecture Map

- `AGENTS_CONTRACT.md` — complete normative Rails API contract.
- `skills/README.md` — skill registry and skill authority rules.
- `skills/context_loading.md` — reusable procedure implementing Phase -1.
- `skills/rails_api_feature.md` — contract-first feature implementation procedure.
- `skills/quality_gate_review.md` — verification evidence procedure when present.
- `skills/flow_prd_update.md` — post-verification derived-doc procedure.
- `docs/CONCEPTS.md` — system terminology and definitions.
- `docs/WORKFLOW.md` — compact execution map.
- `docs/QUALITY_GATES.md` — verification and contract-audit explanation.
- `docs/WORKFLOW_MIGRATION.md` — staged migration plan.
- `changelog.md` — research path, decisions, stage status, and behavior impact.

## Operating Principle

The repository follows a **fat skills, thin harness** model:

- this file stays focused on loading, authority, boundaries, and cross-phase gates
- `AGENTS_CONTRACT.md` preserves the complete normative Rails API contract
- skills provide reusable phase-specific procedures
- verification scripts provide deterministic completion gates

Use progressive disclosure to reduce context noise, never to skip correctness-relevant context.

## Non-Negotiable Entry Rules

- Requirements are source-of-truth inputs and are not rewritten to match implementation.
- Planning-first tasks remain no-code until their confirmation gate is cleared.
- Phase -1 is mandatory before Phase 0.
- Skills are phase-specific execution aids, not independent authorities.
- Activated skills require an invocation record and completion status.
- Missing skills never waive normative workflow requirements.
- `bin/verify` and `bin/contract_audit --all` remain mandatory in the order defined by `AGENTS_CONTRACT.md`.
- Flow/PRD/changelog updates happen only at the stage allowed by the normative contract.
- Final output must include the evidence required by `AGENTS_CONTRACT.md`.

Continue with `AGENTS_CONTRACT.md` after Phase -1 passes.
