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
5. Load additional skills and explanatory docs only when relevant to the current phase.

Do not make code changes before Phase -1 is complete and the normative contract and target requirements have been loaded.

## Authority and Precedence

1. Explicit user instructions govern the requested outcome unless they conflict with a higher-level safety rule.
2. This file defines the repository-level operating and context-loading contract.
3. `AGENTS_CONTRACT.md` defines the complete normative process, engineering rules, verification gates, documentation rules, and final reporting contract.
4. `doc/requirements/**` owns feature/API behavior and numbered requirement versions.
5. `doc/flow/**` and `doc/prd/**` are derived context updated only after verification and compliance gates pass.
6. `skills/**` contains reusable procedures for applying the normative contract; skills cannot override it.
7. `docs/**` is explanatory and adoption-oriented; it is not a second policy authority.

If this file and `AGENTS_CONTRACT.md` appear to conflict, stop and report the conflict. Do not silently choose the less strict rule.

## Phase -1 [NORMATIVE] — Context Loading

Phase -1 runs before Phase 0 in `AGENTS_CONTRACT.md`.

### Goal

Load the minimum **complete** context needed for correct planning while avoiding unrelated bulk context.

Minimum context does not mean partial authority. Every rule, requirement version, dependency, and implementation surface that can affect correctness must be included.

### Context classifications

Classify each loaded source as exactly one primary type:

- `ENTRYPOINT` — this file and mandatory loading/boundary instructions.
- `NORMATIVE` — `AGENTS_CONTRACT.md` sections and other mandatory process rules.
- `SOURCE` — canonical `doc/requirements/**` feature/API contracts.
- `DERIVED` — Flow/PRD/changelog docs that describe verified implementation but do not own behavior.
- `EVIDENCE` — routes, code, schema, specs, generated API docs, migrations, seeds, and configuration.
- `EXPLANATORY` — concepts, workflow guides, migration notes, examples, or other non-authoritative aids.

### Required loading sequence

1. Load this entrypoint.
2. Identify the task, feature label, and requirement path.
3. Load relevant `AGENTS_CONTRACT.md` sections and every normative section they reference.
4. Load the target requirement document and all requirement-owned versions.
5. Locate mirrored Flow/PRD docs and classify them as `DERIVED`.
6. Scan existing implementation evidence:
   - routes
   - controllers
   - models
   - blueprints
   - policies
   - services/queries/jobs when present
   - request, policy, model, blueprint, and rswag specs
   - migrations, schema, seeds, and relevant configuration
7. Expand context only when a discovered dependency, conflict, reference, or ambiguity requires it.
8. Record missing or unresolved context as a gap before planning.

### Progressive disclosure rules

- Do not load every skill, doc, or repository file by default.
- Load a source when it is authoritative for the task, directly referenced, owns a discovered dependency, or is required to resolve ambiguity.
- Requirement documents are never optional when implementing their feature.
- All requirement-owned versions must be considered unless the requirement explicitly states that an older behavior was removed.
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
- source, derived, evidence, and explanatory context are distinguished
- implementation/spec surfaces are scanned sufficiently for planning
- no unresolved context gap can materially change the plan
- no code change has been made

If a material gap remains, output `Ready for Phase 0: NO`, report the gap, and stop before planning.

## Architecture Map

- `AGENTS_CONTRACT.md` — complete normative Rails API contract.
- `skills/README.md` — skill registry and skill authority rules.
- `skills/context_loading.md` — reusable procedure implementing Phase -1.
- `skills/rails_api_feature.md` — contract-first feature implementation procedure.
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
- Skills are execution aids, not independent authorities.
- `bin/verify` and `bin/contract_audit --all` remain mandatory in the order defined by `AGENTS_CONTRACT.md`.
- Flow/PRD/changelog updates happen only at the stage allowed by the normative contract.
- Final output must include the evidence required by `AGENTS_CONTRACT.md`.

Continue with `AGENTS_CONTRACT.md` after Phase -1 passes.
