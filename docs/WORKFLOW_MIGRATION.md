# Workflow Migration Notes

This document records the intended migration path from the current unified contract to a clearer agent-ready workflow system.

The current workflow already works as a contract-first Rails API implementation system. The migration goal is to make the architecture explicit without losing the strictness that makes the toolkit useful.

## Current State

The toolkit currently has:

- one normative contract: `AGENTS.md`
- one fast verification runner: `verify`
- one contract drift auditor: `contract_audit`
- stable Flow and PRD templates
- concepts and workflow overview docs
- an initial skill registry

## Target State

The target state is:

```text
AGENTS.md        -> normative authority and operating harness
skills/**        -> reusable execution procedures
docs/**          -> adoption, concepts, workflow, and quality gate explanations
templates/**     -> structured context schemas
bin/verify       -> verification runner in consumer repos
bin/contract_audit -> contract drift audit in consumer repos
```

## Migration Principle

Do not weaken the existing contract.

The migration should:

- keep `AGENTS.md` normative
- keep requirements as source of truth
- keep Flow and PRD docs derived
- keep verification mandatory
- keep contract audit mandatory
- add reusable skills around the contract
- improve context loading discipline
- make final evidence easier to review

## Stage A: Positioning

Status: in progress in this PR.

Goal:

- describe the repository as an agent-ready contract-first workflow toolkit
- introduce Agent Operating Contract vocabulary
- explain the execution loop clearly

Files:

- `README.md`
- `docs/CONCEPTS.md`
- `docs/WORKFLOW.md`

## Stage B: Quality Gates

Status: in progress in this PR.

Goal:

- explain `bin/verify`
- explain `bin/contract_audit --all`
- define final report evidence expectations
- make verification easier to teach and review

Files:

- `docs/QUALITY_GATES.md`

## Stage C: Skill Registry

Status: in progress in this PR.

Goal:

- move common procedures into explicit reusable skills
- keep `AGENTS.md` as the authority
- reduce repeated task instructions
- improve context discipline

Files:

- `skills/README.md`
- `skills/context_loading.md`
- `skills/rails_api_feature.md`
- `skills/flow_prd_update.md`

## Stage D: AGENTS.md Linking

Status: planned.

Goal:

Add a small introductory note to `AGENTS.md` only.

Proposed content:

```md
It acts as the Agent Operating Contract for Rails API implementation work: a single source for context loading, planning gates, implementation rules, verification gates, documentation updates, and final reporting.

Supporting adoption docs:
- `docs/CONCEPTS.md` defines the agentic workflow vocabulary used by this toolkit.
- `docs/WORKFLOW.md` summarizes the execution loop for teams adopting the contract.
- `docs/QUALITY_GATES.md` explains verification and contract audit gates.
- `skills/README.md` lists reusable execution skills.
```

This stage should not change workflow rules.

## Stage E: Stricter Context Loading

Status: planned.

Potential `AGENTS.md` behavior change:

- make context loading an explicit sub-step before contract extraction
- require agents to list loaded source documents
- require agents to classify docs as source-of-truth or derived context
- require a short context summary before planning

Potential wording:

```text
Before Phase 0 output, list loaded context:
- normative contract sections
- source requirement docs
- derived Flow/PRD docs
- implementation surfaces scanned
- known context gaps
```

This is a logical workflow behavior migration because context engineering is now part of the explicit system.

## Stage F: Skill Invocation

Status: planned.

Potential `AGENTS.md` behavior change:

- reference skills as optional execution aids
- require skills to defer to `AGENTS.md`
- avoid making skills a second authority

Potential wording:

```text
Skills under `skills/**` are reusable procedures for applying this contract.
They are not independent authorities.
If a skill conflicts with AGENTS.md, AGENTS.md wins.
```

## Stage G: Quality Gate Skill

Status: planned.

Goal:

Add a dedicated quality gate skill once file creation is available.

Proposed path:

```text
skills/quality_gate_review.md
```

Purpose:

- collect exact command evidence
- summarize verification results
- prepare final CHECKS and audit sections

## Stage H: README Index Completion

Status: planned.

Goal:

Update README to reference:

- `docs/QUALITY_GATES.md`
- `skills/**`
- `docs/WORKFLOW_MIGRATION.md`

This keeps the repository self-explanatory from the entrypoint.

## What Should Not Change Yet

Do not change yet:

- envelope rules
- status code rules
- docs template rules
- verification command order
- requirement read-only policy
- Flow/PRD version ownership
- final report requirements

Those are already strong and aligned with the target workflow.
