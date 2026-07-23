# Workflow Migration Notes

This document records the migration from the original unified Rails API contract toolkit to an agent-ready workflow system.

The migration principle is unchanged: preserve existing contract discipline, make context and phase ownership explicit, extract reusable procedures, and add enforcement incrementally.

## Architecture After Stage 16

```text
AGENTS.md
  -> small normative entrypoint
  -> Phase -1 context loading
  -> skill invocation and authority boundaries

AGENTS_CONTRACT.md
  -> preserved complete Rails API contract
  -> Phase 0 onward, engineering rules, verification, docs, final evidence

skills/**
  -> phase-owned reusable procedures
  -> canonical structure and explicit handoffs

templates/SKILL_TEMPLATE.md
  -> source authoring schema
  -> installed as doc/templates/SKILL_TEMPLATE.md in consumer repos

verify / contract_audit
  -> deterministic verification and drift gates

docs/**
  -> explanatory and migration guidance
```

## Migration Principles

- do not weaken the original contract
- keep requirements as feature/API source of truth
- keep Flow/PRD docs derived
- keep verification and contract audit mandatory
- use progressive context disclosure without omitting dependencies
- give each skill one primary phase responsibility
- carry artifacts and decisions between phases, not unnecessary procedure text
- add automated enforcement only after the documented structure stabilizes

## Completed Migration Stages

### Stage A — Positioning and Vocabulary

**Status:** Done

Implemented:

- agent-ready README positioning
- Agent Operating Contract terminology
- `docs/CONCEPTS.md`
- `docs/WORKFLOW.md`

### Stage B — Quality-Gate Model

**Status:** Done

Implemented:

- `docs/QUALITY_GATES.md`
- explicit verification/audit evidence model
- `QUALITY GATE DECISION: PASS/BLOCKED`

### Stage C — Initial Skill Registry

**Status:** Done

Implemented:

- `skills/README.md`
- `skills/context_loading.md`
- `skills/rails_api_feature.md`
- `skills/quality_gate_review.md`
- `skills/flow_prd_update.md`

### Stage D — AGENTS Orientation Split

**Status:** Done

Implemented:

- small root `AGENTS.md` entrypoint/harness
- original full contract preserved as `AGENTS_CONTRACT.md`
- mandatory authority and load-order boundaries

### Stage E — Stricter Context Loading

**Status:** Done

Implemented normative Phase -1:

- source classifications
- `CONTEXT INVENTORY`
- `CONTEXT SUMMARY`
- dependency-driven expansion
- `Ready for Phase 0: YES/NO`
- material-gap stop gate

### Stage F — Skill Invocation Contract

**Status:** Done

Implemented:

- activation rules
- `PROCEDURE` context classification
- invocation lifecycle
- statuses: `ACTIVATED`, `COMPLETED`, `BLOCKED`, `NOT_REQUIRED`, `UNAVAILABLE`
- precedence and fallback rules
- phase-by-phase loading boundaries

### Stage G — Quality Gate Skill

**Status:** Done

Implemented:

- exact command/evidence collection
- profile-selection rationale
- compliance and traceability review
- explicit derived-doc readiness decision

### Stage H — README Consolidation

**Status:** Done

README now exposes:

- authority model
- Phase -1
- skill invocation
- quality-gate decision
- complete docs/skills/templates map
- consumer installation layout

### Stage I — Skill Consistency Audit

**Status:** Done

Implemented:

- `templates/SKILL_TEMPLATE.md`
- canonical section order for every skill
- one primary phase owner per skill
- stable output and handoff for each skill
- normalized registry ownership map
- reduced overlap between implementation, verification, and docs skills

Ownership chain:

```text
context_loading
  -> rails_api_feature
  -> quality_gate_review
  -> flow_prd_update when required
  -> final report
```

## Preserved Invariants

The migration did not intentionally alter:

- API envelope and status-code rules
- Blueprinter ownership
- authorization/search/pagination conventions
- requirement read-only policy
- requirement-owned versions
- Flow/PRD structure and ownership rules
- verification command order
- final evidence requirements

## Remaining Stage J — Automated Skills and Docs Audit

**Status:** Next

Goal: enforce the stabilized structure mechanically.

Planned checks:

- required skill headings and order
- authority statement presence
- unique skill titles and ownership
- registry entries match skill files
- README references exist
- local Markdown/script references resolve
- required contract/template files exist
- expected consumer-installation mappings are documented

The preferred implementation is a focused supplemental audit tool rather than unsafe large edits to the existing mature `contract_audit` core.

## Remaining Stage K — Final Integration Audit

**Status:** Planned

Final work:

- run repository-wide consistency review
- verify docs, contracts, skills, templates, and tooling agree
- update PR title/body to final scope
- synchronize changelog and migration status
- document checks that were run and checks unavailable in this repository-only environment
- leave the PR ready for owner review/merge without merging automatically
