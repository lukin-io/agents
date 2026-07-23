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
2. This changelog records:
   - status
   - files changed
   - what changed
   - why
   - behavior impact
   - decisions/limitations
   - next stage
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

It already provided strong workflow discipline:

- contract-first implementation against `doc/requirements/**`
- requirement-owned API behavior and version authority
- no-code planning phases
- explicit implementation confirmation gate
- canonical success and error envelopes
- Blueprinter-owned `data` payloads
- Pundit authorization
- Ransack filtering/search
- Kaminari pagination
- repeatable pre-merge verification
- contract-drift detection
- stable Flow/PRD schemas
- requirement-to-code traceability
- final evidence reporting

The gap was not the absence of an engineering workflow. The gap was that its structure and public description did not explicitly map the existing mechanics to agent engineering, context engineering, reusable skills, verification-driven development, or AI-assisted delivery.

---

# 2. Research Path

## 2.1 Industry Direction — What Is Changing

Resources:

- https://youtu.be/igO8iyca2_g?t=1
- https://youtu.be/_R83pFpUWyM?t=1
- https://youtu.be/5ID22ACI7IM?t=1
- https://x.com/garrytan/status/2053127519872614419?s=20

Conclusions:

- AI-assisted work is moving from single responses toward repeatable execution systems.
- One-off prompts do not compound reliably across teams and repositories.
- Reusable workflows and skills are more valuable than repeatedly authored large prompts.
- A useful architecture is **fat skills, thin harness**.
- Generation becomes cheaper; verification becomes more important.
- Durable value comes from context, workflow, knowledge, verification, and traceability—not only model choice.

## 2.2 Agent Platforms — How It Is Built

Resources:

- https://openai.com/solutions/use-case/agents/
- https://developers.openai.com/api/docs/guides/agent-builder
- https://developers.openai.com/api/docs/guides/agents
- https://openai.com/uk-UA/agent-platform/

Conclusions:

- An agent is an execution loop, not only a generated response.
- Tools, state, handoffs, approvals, traces, and evaluations are system components.
- Production workflows require observability and evaluation.
- Multi-agent architecture should only be introduced for useful responsibility boundaries.
- A reliable single workflow is often better than premature orchestration complexity.

## 2.3 Context and Reliability — How It Works Consistently

Resources:

- https://x.com/Kappaemme1926/status/2050908233158816122
- https://www.youtube.com/watch?v=esY99nYXxR4

Conclusions:

- Context quality matters more than context volume.
- A context window is capacity, not guaranteed understanding.
- Context should be loaded deliberately and progressively.
- A tool is a capability; a skill is a procedure for using capabilities correctly.
- Large universal prompts accumulate contradictions and noise.
- Reusable procedures should define inputs, steps, outputs, checks, and failure modes.
- Verification and traceability are essential to reliable execution.

## 2.4 Internal Evidence

The direction was compared against existing implementation practice and published experience:

- https://lukin.io/blog/mergeable-by-default-context-engine-ai-codegen/
- https://lukin.io/blog/1-backend-engineer-vs-11-engineers-with-ai/

Internal principles already demonstrated:

- generated work should be mergeable by default
- ambiguity should be reduced before implementation
- repository conventions should be explicit
- verification should be deterministic
- context should be treated as engineering infrastructure
- requirements, code, specs, and docs should remain traceable
- delivery speed should increase without weakening quality gates

The migration therefore exposes, modularizes, and strengthens the existing system instead of replacing it with unnecessary orchestration.

---

# 3. Target Architecture

```text
AGENTS.md
  -> small mandatory entrypoint
  -> load order, authority, boundaries, system map

AGENTS_CONTRACT.md
  -> complete normative Rails API workflow contract
  -> process, invariants, verification, docs, final evidence

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
requirement handoff
  -> context loading
  -> contract extraction
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

Architectural principle:

```text
fat skills, thin harness
```

---

# 4. Completed Stages

## Stage 1 — Agent-Ready Positioning

**Status:** Done

**Primary file:** `README.md`

### Implemented

- Reframed the repository as an agent-ready, contract-first workflow toolkit.
- Introduced Agent Operating Contract terminology.
- Documented the end-to-end execution chain.
- Explained support for human engineers and coding agents.
- Documented prompt drift, context pollution, contract drift, verification gaps, and unreviewable output as target problems.

### Why

The system already behaved as an agent-ready workflow but appeared narrower than it was.

### Behavior impact

Positioning and documentation only.

---

## Stage 2 — Concepts and Shared Vocabulary

**Status:** Done

**File:** `docs/CONCEPTS.md`

### Implemented

Defined:

- Agent Operating Contract
- Source-of-Truth Context
- Derived Context Docs
- Context Engineering
- Contract-First Execution
- Planning-First Gate
- Verification-Driven Development
- Contract Drift Audit
- Contract Traceability Matrix
- Discrepancy Taxonomy
- Structured Context Templates

### Why

Terminology is useful only when mapped to concrete files, authority, outputs, and gates.

### Behavior impact

Explanatory foundation for later normative changes.

---

## Stage 3 — Workflow Adoption Guide

**Status:** Done

**File:** `docs/WORKFLOW.md`

### Implemented

Documented requirement handoff through final evidence and separated human decision responsibilities from procedures suitable for coding agents.

### Why

A concise adoption map improves onboarding without turning explanatory docs into policy.

### Behavior impact

Explanatory only.

---

## Stage 4 — Quality Gates Documentation

**Status:** Done

**File:** `docs/QUALITY_GATES.md`

### Implemented

Unified:

```text
contract alignment
  -> bin/verify
  -> bin/contract_audit --all
  -> derived docs/changelog
  -> final evidence
```

Documented fast/full profiles, audit scope, requirement handoff exception, and exact `CHECKS` reporting.

### Why

Adopters needed one explanation of why both verification gates exist and when documentation updates become allowed.

### Behavior impact

Existing command order unchanged.

---

## Stage 5 — Initial Skill Registry

**Status:** Done

**File:** `skills/README.md`

### Implemented

Defined skills by activation conditions, inputs, steps, outputs, checks, failure modes, and handoffs.

Authority rule:

```text
AGENTS contract layers remain normative.
skills/** applies the contract.
Skills cannot override the contract or requirements.
```

### Why

Reusable procedures should not be reconstructed from ad-hoc prompts.

### Behavior impact

Introduced a modular procedure layer; normative invocation rules remain a later stage.

---

## Stage 6 — Context Loading Skill

**Status:** Done

**File:** `skills/context_loading.md`

### Implemented

- relevant normative loading
- all target requirement versions
- derived-doc classification
- implementation/spec scan
- context gaps
- compact `CONTEXT SUMMARY`

### Why

Context engineering should be explicit rather than “read the repo.”

### Behavior impact

Reusable procedure; normative enforcement is Stage 13.

---

## Stage 7 — Rails API Feature Skill

**Status:** Done

**File:** `skills/rails_api_feature.md`

### Implemented

Extracted contract extraction, scan, plan, confirmation, implementation, verification, docs, and final evidence into one reusable feature skill.

### Why

Feature implementation is the central repeated workflow.

### Behavior impact

Procedure only; normative authority remains in the AGENTS contract.

---

## Stage 8 — Flow/PRD Update Skill

**Status:** Done

**File:** `skills/flow_prd_update.md`

### Implemented

Defined post-verification Flow/PRD update rules, mirrored paths, requirement-owned versions, traceability, migration impact, and Drift Delta.

### Why

Derived documentation has different authority and timing from implementation.

### Behavior impact

Procedure only.

---

## Stage 9 — Controlled Migration Plan

**Status:** Done

**File:** `docs/WORKFLOW_MIGRATION.md`

### Implemented

Documented positioning, quality docs, skill extraction, AGENTS orientation, context behavior, skill invocation, quality skill, README completion, and later enforcement.

### Why

The normative workflow should evolve through controlled stages, not a large uncontrolled rewrite.

### Behavior impact

Migration governance only.

---

## Stage 10 — Historical and Decision Changelog

**Status:** Done

**File:** `changelog.md`

### Implemented

Recorded the original system, resources, extracted concepts, internal evidence, target architecture, stages, decisions, and remaining work.

### Why

The umbrella PR needs a durable explanation of how and why the system evolved.

### Behavior impact

Documentation governance.

---

## Stage 11 — README Consolidation

**Status:** Done

**Implementation commit:** `8586de6e9978c2c254cdd2513b3367e78cbcbdec`

**Changelog commit:** `ef30039730ccad77757a4320c20c10d63e138b49`

### Implemented

README became the complete repository entrypoint with:

- architecture overview
- authority model
- all docs and skills
- progressive loading order
- installation layout
- repository tree
- terminology and non-goals

### Why

New docs and skills were otherwise hidden from users entering through README.

### Behavior impact

No normative behavior change; adoption and progressive disclosure became explicit.

---

## Stage 12 — AGENTS Orientation Layer

**Status:** Done

**Orientation/core commit:** `b2746c8da7ca57f649f40de9a3dcd658e63ccdde`

**README synchronization commit:** `452fd707456cfa10a2f6050eabecfd3b73b93f6e`

**Changelog synchronization:** this commit

**Files:**

- `AGENTS.md`
- `AGENTS_CONTRACT.md`
- `README.md`
- `changelog.md`

### Implemented

The former full `AGENTS.md` was preserved without modification as:

```text
AGENTS_CONTRACT.md
```

The existing blob was reused directly, avoiding truncation or semantic rewrite.

A new small root `AGENTS.md` now defines:

- mandatory load order
- repository entry behavior
- authority and precedence boundaries
- conflict handling
- architecture map
- thin-harness operating principle
- progressive disclosure guidance
- non-negotiable entry rules
- mandatory transition into `AGENTS_CONTRACT.md`

README was updated to describe the two-layer normative system:

```text
AGENTS.md
  -> entrypoint, load order, authority, boundaries

AGENTS_CONTRACT.md
  -> complete normative Rails API workflow contract
```

### Why

The original file was more than 1,100 lines. It mixed the unavoidable repository entrypoint with the full detailed contract and illustrative patterns. Every session risked loading the entire file even when only a small portion was relevant.

The split creates a small predictable entrypoint while preserving every original normative rule and example exactly. It also avoids the earlier connector failure mode where a partial file fetch caused accidental deletion of the unseen tail.

### Behavior impact

This is the first structural runtime-context change:

- agents now encounter a concise root entrypoint
- the root file requires loading the complete normative contract or all relevant sections and dependencies before work
- authority between docs, skills, requirements, and derived docs is explicit
- the original contract remains intact and mandatory

No original engineering rule, verification gate, envelope rule, documentation rule, or final-output requirement was removed.

### Decisions

- The root file stays small to support a thin harness.
- `AGENTS_CONTRACT.md` remains normative, not explanatory documentation.
- The split is not an attempt to avoid loading required rules.
- Progressive disclosure may reduce unrelated context, but correctness dependencies must always be loaded.
- Conflict between the two contract layers is a stop condition.

### Next stage

Stage 13: make context loading a complete normative phase with source classification, `CONTEXT SUMMARY`, context-gap reporting, and progressive-disclosure rules.

---

# 5. Remaining Stages

## Stage 13 — Stricter Context Loading Behavior

**Status:** Next

Planned normative changes:

- explicit context-loading phase before contract extraction
- required loaded-source inventory
- authority classification:
  - entrypoint/normative contract
  - source-of-truth requirement
  - derived context
  - implementation evidence
  - optional explanatory context
- required `CONTEXT SUMMARY`
- required context-gap reporting
- progressive disclosure and dependency expansion
- no unrelated bulk context loading

## Stage 14 — Skill Invocation Contract

**Status:** Planned

Planned normative changes:

- skill activation rules
- required inputs and outputs
- skill precedence
- no bulk-loading all skills
- skills reference normative rules instead of duplicating them
- conflict protocol

## Stage 15 — Quality Gate Review Skill

**Status:** Planned

Planned file:

```text
skills/quality_gate_review.md
```

Responsibilities:

- contract-alignment evidence
- exact verification commands
- exit codes
- failure summaries
- discrepancy report
- readiness decision for derived docs

## Stage 16 — Skill Consistency Audit

**Status:** Planned

- normalize structure
- remove unnecessary normative duplication
- verify terminology, inputs, outputs, and failure modes
- consider `templates/SKILL_TEMPLATE.md`

## Stage 17 — Automated Skills and Docs Audit

**Status:** Planned

Potential `contract_audit` additions:

- required skill headings
- authority declarations
- internal path/link checks
- README index completeness
- docs referencing missing files
- invalid or duplicate skill ownership

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

# 6. Current Repository State After Stage 12

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

The repository now has a small mandatory entrypoint, a preserved complete normative contract, explanatory documentation, an initial skill registry, documented quality gates, and a stage-governed migration path.
