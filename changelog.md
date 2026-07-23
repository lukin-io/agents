# Agentic Workflow Alignment Changelog

This document records how `lukin-io/agents` is evolving from a strict Rails API contract toolkit into an explicitly agent-ready engineering workflow. It explains the research path, architectural reasoning, completed stages in PR #3, behavior impact, decisions, and remaining migration work.

## Change Scope

- Repository: `lukin-io/agents`
- Branch: `chore/agentic_update`
- Pull request: `#3`
- Base branch: `main`
- Work period: May 10, 2026 onward
- Current update date: July 23, 2026
- Change type: positioning, documentation alignment, skill extraction, context engineering, verification-driven workflow migration, and enforcement preparation

PR #3 is intentionally an umbrella pull request. Each logical stage is committed separately so it can be reviewed, adjusted, or reverted independently.

## Stage Completion Rule

A stage is complete only after:

1. Stage implementation is committed and pushed.
2. This changelog records status, files, what changed, why, behavior impact, decisions/limitations, and next stage.
3. The changelog update is committed and pushed separately.
4. A concise stage summary is provided to the project owner.

---

# 1. Starting Point

The original repository contained:

```text
AGENTS.md
verify
contract_audit
templates/
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

It already provided:

- contract-first implementation against `doc/requirements/**`
- requirement-owned API behavior and version authority
- no-code planning and confirmation gates
- canonical success and error envelopes
- Blueprinter-owned `data` payloads
- Pundit, Ransack, and Kaminari conventions
- repeatable pre-merge verification
- contract-drift detection
- stable Flow/PRD schemas
- requirement-to-code traceability
- final evidence reporting

The gap was not missing discipline. The gap was that the public structure did not explicitly map those mechanics to agent engineering, context engineering, reusable skills, verification-driven development, or AI-assisted delivery.

---

# 2. Research Path

## 2.1 Industry Direction

Resources:

- https://youtu.be/igO8iyca2_g?t=1
- https://youtu.be/_R83pFpUWyM?t=1
- https://youtu.be/5ID22ACI7IM?t=1
- https://x.com/garrytan/status/2053127519872614419?s=20

Conclusions:

- AI-assisted work is moving from single responses toward repeatable execution systems.
- One-off prompts do not compound reliably across teams and repositories.
- Reusable workflows and skills are more valuable than repeatedly authored large prompts.
- **Fat skills, thin harness** is a useful architecture.
- Generation becomes cheaper; verification becomes more important.
- Durable value comes from context, workflow, knowledge, verification, and traceability.

## 2.2 Agent Platforms

Resources:

- https://openai.com/solutions/use-case/agents/
- https://developers.openai.com/api/docs/guides/agent-builder
- https://developers.openai.com/api/docs/guides/agents
- https://openai.com/uk-UA/agent-platform/

Conclusions:

- An agent is an execution loop, not only a generated response.
- Tools, state, handoffs, approvals, traces, and evaluations are system components.
- Production workflows require observability and evaluation.
- Multi-agent systems should only be introduced for useful responsibility boundaries.
- A reliable single workflow is often better than premature orchestration complexity.

## 2.3 Context and Reliability

Resources:

- https://x.com/Kappaemme1926/status/2050908233158816122
- https://www.youtube.com/watch?v=esY99nYXxR4

Conclusions:

- Context quality matters more than context volume.
- A context window is capacity, not guaranteed understanding.
- Context should be loaded deliberately and progressively.
- A tool is a capability; a skill is a procedure for using capabilities correctly.
- Large universal prompts accumulate contradictions and noise.
- Skills should define inputs, steps, outputs, checks, and failure modes.
- Verification and traceability are essential to reliable execution.

## 2.4 Internal Evidence

Resources:

- https://lukin.io/blog/mergeable-by-default-context-engine-ai-codegen/
- https://lukin.io/blog/1-backend-engineer-vs-11-engineers-with-ai/

Principles already demonstrated internally:

- generated work should be mergeable by default
- ambiguity should be reduced before implementation
- repository conventions should be explicit
- verification should be deterministic
- context should be treated as engineering infrastructure
- requirements, code, specs, and docs should remain traceable
- delivery speed should increase without weakening quality gates

The migration therefore exposes, modularizes, and strengthens the existing system instead of replacing it with unnecessary orchestration.

---

# 3. Current Target Architecture

```text
AGENTS.md
  -> small mandatory entrypoint
  -> Phase -1 context loading
  -> authority, boundaries, cross-phase gates

AGENTS_CONTRACT.md
  -> preserved complete Rails API contract
  -> Phase 0 onward, engineering invariants, verification, docs, final evidence

skills/**
  -> reusable phase-specific procedures
  -> progressively loaded when relevant

verify
  -> source for bin/verify
  -> deterministic verification profiles

contract_audit
  -> source for bin/contract_audit
  -> static drift and compliance checks

docs/**
  -> explanatory, adoption, quality, and migration documentation
  -> never a second normative authority

templates/**
  -> stable schemas for derived Flow and PRD context

changelog.md
  -> research path, decisions, stages, behavior impact, remaining work
```

Execution loop:

```text
Phase -1 context loading
  -> requirement handoff/identification
  -> Phase 0 contract extraction
  -> repository scan
  -> implementation plan
  -> confirmation gate
  -> implementation
  -> contract alignment
  -> verification
  -> contract audit
  -> derived documentation
  -> final evidence report
```

---

# 4. Completed Stages

## Stage 1 — Agent-Ready Positioning

**Status:** Done

**Primary file:** `README.md`

Implemented agent-ready positioning, Agent Operating Contract terminology, execution-chain overview, and explicit problem framing.

**Behavior impact:** documentation only.

---

## Stage 2 — Concepts and Shared Vocabulary

**Status:** Done

**File:** `docs/CONCEPTS.md`

Defined Agent Operating Contract, Source-of-Truth Context, Derived Context Docs, Context Engineering, Contract-First Execution, Planning-First Gate, Verification-Driven Development, Contract Drift Audit, traceability, discrepancy taxonomy, and structured context schemas.

**Behavior impact:** explanatory foundation.

---

## Stage 3 — Workflow Adoption Guide

**Status:** Done

**File:** `docs/WORKFLOW.md`

Documented the compact execution map and human/tool responsibility boundaries.

**Behavior impact:** explanatory only.

---

## Stage 4 — Quality Gates Documentation

**Status:** Done

**File:** `docs/QUALITY_GATES.md`

Unified contract alignment, `bin/verify`, `bin/contract_audit --all`, derived docs timing, and final evidence into one gate model.

**Behavior impact:** existing command behavior unchanged.

---

## Stage 5 — Initial Skill Registry

**Status:** Done

**File:** `skills/README.md`

Defined reusable skills by activation, inputs, steps, outputs, checks, failure modes, and authority boundaries.

**Behavior impact:** introduced modular procedures without changing normative invocation.

---

## Stage 6 — Context Loading Skill

**Status:** Done

**File:** `skills/context_loading.md`

Created a reusable context-loading procedure and initial `CONTEXT SUMMARY` artifact.

**Behavior impact:** procedure layer; later made normative in Stage 13.

---

## Stage 7 — Rails API Feature Skill

**Status:** Done

**File:** `skills/rails_api_feature.md`

Extracted contract extraction, repo scan, planning, confirmation, implementation, verification, docs, and final evidence into a reusable skill.

**Behavior impact:** procedure layer.

---

## Stage 8 — Flow/PRD Update Skill

**Status:** Done

**File:** `skills/flow_prd_update.md`

Created a post-verification procedure for mirrored paths, requirement-owned versions, traceability, migration impact, and Drift Delta.

**Behavior impact:** procedure layer.

---

## Stage 9 — Controlled Migration Plan

**Status:** Done

**File:** `docs/WORKFLOW_MIGRATION.md`

Documented the safe staged migration and invariants that should not change casually.

**Behavior impact:** migration governance.

---

## Stage 10 — Historical and Decision Changelog

**Status:** Done

**File:** `changelog.md`

Recorded resources, reasoning, architecture, stages, decisions, and remaining work.

**Behavior impact:** documentation governance.

---

## Stage 11 — README Consolidation

**Status:** Done

**Implementation commit:** `8586de6e9978c2c254cdd2513b3367e78cbcbdec`

**Changelog commit:** `ef30039730ccad77757a4320c20c10d63e138b49`

README became the complete entrypoint with architecture, authority, docs/skills index, progressive loading, installation, terminology, and non-goals.

**Behavior impact:** adoption behavior clarified; normative rules unchanged.

---

## Stage 12 — AGENTS Orientation Layer

**Status:** Done

**Orientation/core commit:** `b2746c8da7ca57f649f40de9a3dcd658e63ccdde`

**README commit:** `452fd707456cfa10a2f6050eabecfd3b73b93f6e`

**Changelog commit:** `d5c80ce85ada191c7ca97316581a2f06f271d5cd`

### Implemented

- Preserved the former full `AGENTS.md` byte-for-byte as `AGENTS_CONTRACT.md`.
- Replaced root `AGENTS.md` with a small mandatory entrypoint/harness.
- Added load order, authority boundaries, architecture map, conflict handling, progressive disclosure guidance, and mandatory transition into the full contract.
- Updated README to describe the two-layer normative system.

### Why

The original 1,100+ line file mixed the unavoidable entrypoint with the complete detailed contract and illustrative examples. Splitting it reduces default context pressure while preserving every rule.

### Behavior impact

First structural runtime-context change. No original contract rule was removed.

---

## Stage 13 — Stricter Context Loading Behavior

**Status:** Done

**Normative Phase -1 commit:** `7f56f16d26799815e6d03174cd157a4c832a847a`

**Context skill commit:** `6f3089e6abb953f6923fd6e6fda943090af72a23`

**README commit:** `716d2c3ef851d2226ef49b07d10c36fed2907825`

**Changelog synchronization:** this commit

**Files:**

- `AGENTS.md`
- `skills/context_loading.md`
- `README.md`
- `changelog.md`

### Implemented

Added normative **Phase -1: Context Loading** before Phase 0.

The root contract now requires each loaded source to be classified as:

- `ENTRYPOINT`
- `NORMATIVE`
- `SOURCE`
- `DERIVED`
- `EVIDENCE`
- `EXPLANATORY`

Added a mandatory loading sequence:

1. Load the entrypoint.
2. Identify task, feature, and requirement path.
3. Load relevant complete-contract sections and referenced normative dependencies.
4. Load the target requirement and all requirement-owned versions.
5. Locate mirrored derived docs.
6. Scan implementation and spec evidence.
7. Expand context only for discovered dependencies, conflicts, references, or ambiguity.
8. Record unresolved gaps before planning.

Added progressive-disclosure rules:

- do not bulk-load every skill/doc/file
- load authoritative or directly relevant context
- requirements are mandatory
- derived/explanatory docs cannot override authority
- evidence describes current implementation but cannot redefine requirements
- update the inventory when later investigation adds context

Added mandatory artifacts:

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

Added a Phase -1 completion gate. If a material context gap can change the plan, output `Ready for Phase 0: NO` and stop before planning.

Updated `skills/context_loading.md` to implement exactly the same classifications, sequence, outputs, completion checks, and failure modes.

Updated README to expose the normative Phase -1 behavior to adopters.

### Why

Before this stage, context loading was documented and available as a skill but not mandatory. Agents could still jump directly into contract extraction with implicit or incomplete context.

This stage turns context engineering into an enforceable workflow phase. It addresses context pollution and missing-context risk simultaneously:

- unrelated context is not loaded automatically
- correctness-relevant context cannot be skipped
- authority differences are explicit
- planning readiness becomes reviewable

### Behavior impact

This is a normative workflow behavior migration:

- Phase -1 is mandatory before Phase 0
- no code changes are allowed during Phase -1
- a context inventory and summary are required
- missing material context blocks planning
- progressive disclosure is mandatory but cannot be used to omit dependencies

### Decisions

- Context classification uses one primary type per source to keep inventories compact.
- Requirement versions remain mandatory even under progressive disclosure.
- `AGENTS_CONTRACT.md` Phase 0 remains intact; Phase -1 precedes it rather than rewriting it.
- Skill invocation itself remains the next stage; Stage 13 defines the behavior independent of a specific skill file.

### Next stage

Stage 14: define the normative skill invocation contract—activation, inputs, outputs, precedence, progressive loading, and conflict behavior.

---

# 5. Remaining Stages

## Stage 14 — Skill Invocation Contract

**Status:** Next

Planned:

- skill activation rules
- required inputs and outputs
- skill precedence
- no bulk-loading all skills
- normative references instead of duplicated policy
- skill conflict and fallback protocol

## Stage 15 — Quality Gate Review Skill

**Status:** Planned

Planned file:

```text
skills/quality_gate_review.md
```

Responsibilities:

- contract-alignment evidence
- exact verification commands and exit codes
- failure summary
- discrepancy report
- readiness decision for derived docs

## Stage 16 — Skill Consistency Audit

**Status:** Planned

- normalize skill structure
- reduce normative duplication
- verify terminology, inputs, outputs, and failure modes
- introduce `templates/SKILL_TEMPLATE.md` if justified

## Stage 17 — Automated Skills and Docs Audit

**Status:** Planned

Potential `contract_audit` additions:

- required skill headings
- authority declarations
- internal path/link checks
- README index completeness
- missing referenced files
- duplicate skill ownership

## Stage 18 — Final Integration Audit

**Status:** Planned

- README/docs/skills/AGENTS consistency
- no duplicate authorities
- no contradictory workflow stages
- explicit context loading
- progressive skill loading
- mandatory verification
- scripts/docs alignment
- synchronized changelog
- final PR description

---

# 6. Current Repository State After Stage 13

```text
AGENTS.md
AGENTS_CONTRACT.md
README.md
changelog.md
verify
contract_audit
docs/
  CONCEPTS.md
  QUALITY_GATES.md
  WORKFLOW.md
  WORKFLOW_MIGRATION.md
skills/
  README.md
  context_loading.md
  rails_api_feature.md
  flow_prd_update.md
templates/
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

The repository now has a small mandatory entrypoint, a preserved complete contract, normative context loading, explicit source classification, reusable skills, documented quality gates, and a stage-governed migration path.
