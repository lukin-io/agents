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
2. This changelog records status, files, changes, rationale, behavior impact, decisions/limitations, and next stage.
3. The changelog update is committed and pushed separately.
4. A concise stage summary is provided to the project owner.

---

# 1. Starting Point

The repository began as a compact Rails API enforcement toolkit:

```text
AGENTS.md
verify
contract_audit
templates/
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

It already enforced contract-first implementation, requirement-owned behavior, no-code planning, confirmation gates, canonical envelopes, Blueprinter ownership, Pundit/Ransack/Kaminari conventions, repeatable verification, drift detection, structured derived docs, traceability, and final evidence.

The migration goal is not to replace that discipline. It is to expose, modularize, and strengthen it using agent engineering, context engineering, reusable skills, and verification-driven workflow concepts.

---

# 2. Research Path

## 2.1 Industry Direction

Resources:

- https://youtu.be/igO8iyca2_g?t=1
- https://youtu.be/_R83pFpUWyM?t=1
- https://youtu.be/5ID22ACI7IM?t=1
- https://x.com/garrytan/status/2053127519872614419?s=20

Conclusions:

- AI-assisted work is moving toward repeatable execution systems.
- One-off prompts do not compound reliably.
- Reusable workflows and skills are more valuable than large repeated prompts.
- **Fat skills, thin harness** is a useful architecture.
- Verification becomes more important as generation becomes cheaper.

## 2.2 Agent Platforms

Resources:

- https://openai.com/solutions/use-case/agents/
- https://developers.openai.com/api/docs/guides/agent-builder
- https://developers.openai.com/api/docs/guides/agents
- https://openai.com/uk-UA/agent-platform/

Conclusions:

- An agent is an execution loop, not only a response.
- Tools, state, handoffs, approvals, traces, and evaluations are system components.
- Production workflows require observability and evaluation.
- Multi-agent systems should only be introduced for useful responsibility boundaries.

## 2.3 Context and Reliability

Resources:

- https://x.com/Kappaemme1926/status/2050908233158816122
- https://www.youtube.com/watch?v=esY99nYXxR4

Conclusions:

- Context quality matters more than context volume.
- Context should be loaded deliberately and progressively.
- A tool is a capability; a skill is a repeatable procedure.
- Skills need inputs, steps, outputs, checks, and failure modes.
- Verification and traceability are essential.

## 2.4 Internal Evidence

Resources:

- https://lukin.io/blog/mergeable-by-default-context-engine-ai-codegen/
- https://lukin.io/blog/1-backend-engineer-vs-11-engineers-with-ai/

Existing principles:

- generated work should be mergeable by default
- ambiguity should be reduced before implementation
- repository conventions should be explicit
- verification should be deterministic
- context should be engineering infrastructure
- requirements, code, specs, and docs should remain traceable

---

# 3. Current Architecture

```text
AGENTS.md
  -> small mandatory entrypoint
  -> Phase -1 context loading
  -> skill invocation contract
  -> authority, boundaries, cross-phase gates

AGENTS_CONTRACT.md
  -> preserved complete Rails API contract
  -> Phase 0 onward, engineering invariants, verification, docs, final evidence

skills/**
  -> reusable phase-specific procedures
  -> progressively activated when relevant

verify
  -> deterministic verification profiles

contract_audit
  -> static drift and compliance checks

docs/**
  -> explanatory and migration documentation
  -> never a second normative authority

templates/**
  -> stable schemas for derived context
```

Execution loop:

```text
Phase -1 context loading
  -> skill activation
  -> Phase 0 contract extraction
  -> repository scan
  -> plan
  -> confirmation
  -> implementation
  -> contract alignment
  -> verification
  -> contract audit
  -> derived docs
  -> final evidence
```

---

# 4. Stage Status

| Stage | Status | Main outcome |
| --- | --- | --- |
| 1. Positioning | Done | Agent-ready repository framing |
| 2. Concepts | Done | Stable vocabulary |
| 3. Workflow guide | Done | Compact adoption map |
| 4. Quality gates docs | Done | Unified verification model |
| 5. Skill registry | Done | Reusable procedure layer |
| 6. Context skill | Done | Explicit context-loading procedure |
| 7. Rails feature skill | Done | Reusable implementation lifecycle |
| 8. Flow/PRD skill | Done | Post-verification docs procedure |
| 9. Migration plan | Done | Controlled behavior migration |
| 10. Historical changelog | Done | Research and decision record |
| 11. README consolidation | Done | Complete public entrypoint |
| 12. AGENTS orientation | Done | Thin entrypoint + preserved contract core |
| 13. Normative context loading | Done | Mandatory Phase -1 and readiness gate |
| 14. Skill invocation contract | Done | Activation, precedence, lifecycle, status, fallback |
| 15. Quality gate review skill | Next | Reusable verification evidence procedure |
| 16. Skill consistency audit | Planned | Normalized skill schema and reduced duplication |
| 17. Automated audit | Planned | Machine-enforced skills/docs integrity |
| 18. Final integration | Planned | Full consistency and PR completion |

---

# 5. Completed Stage Details

## Stages 1–4 — Positioning, Concepts, Workflow, Quality Gates

**Files:**

- `README.md`
- `docs/CONCEPTS.md`
- `docs/WORKFLOW.md`
- `docs/QUALITY_GATES.md`

Implemented agent-ready positioning, stable terminology, compact execution guidance, human/tool boundaries, and a unified verification model.

**Behavior impact:** explanatory; existing contract behavior remained intact.

---

## Stages 5–8 — Initial Skills

**Files:**

- `skills/README.md`
- `skills/context_loading.md`
- `skills/rails_api_feature.md`
- `skills/flow_prd_update.md`

Extracted reusable procedures for context loading, Rails API implementation, and post-verification derived docs.

**Behavior impact:** modular procedure layer; authority remained in the contract.

---

## Stages 9–10 — Migration Governance and Changelog

**Files:**

- `docs/WORKFLOW_MIGRATION.md`
- `changelog.md`

Established a controlled migration sequence and durable record of research, decisions, behavior impact, and next work.

---

## Stage 11 — README Consolidation

**Status:** Done

**Implementation commit:** `8586de6e9978c2c254cdd2513b3367e78cbcbdec`

**Changelog commit:** `ef30039730ccad77757a4320c20c10d63e138b49`

README became the complete entrypoint with architecture, authority, all docs/skills, loading order, installation, terminology, and non-goals.

**Behavior impact:** adoption clarified; normative rules unchanged.

---

## Stage 12 — AGENTS Orientation Layer

**Status:** Done

**Orientation/core commit:** `b2746c8da7ca57f649f40de9a3dcd658e63ccdde`

**README commit:** `452fd707456cfa10a2f6050eabecfd3b73b93f6e`

**Changelog commit:** `d5c80ce85ada191c7ca97316581a2f06f271d5cd`

### Implemented

- Preserved the former full `AGENTS.md` byte-for-byte as `AGENTS_CONTRACT.md`.
- Replaced root `AGENTS.md` with a small mandatory entrypoint/harness.
- Added load order, authority boundaries, architecture map, conflict handling, and progressive-disclosure guidance.

### Why

The original 1,100+ line file mixed the unavoidable entrypoint with the complete detailed contract. The split reduces default context pressure while preserving every original rule.

### Behavior impact

First structural runtime-context change. No original contract rule was removed.

---

## Stage 13 — Stricter Context Loading Behavior

**Status:** Done

**Phase -1 commit:** `7f56f16d26799815e6d03174cd157a4c832a847a`

**Context skill commit:** `6f3089e6abb953f6923fd6e6fda943090af72a23`

**README commit:** `716d2c3ef851d2226ef49b07d10c36fed2907825`

**Changelog commit:** `140b1e6383aa1afea42b9e1f6b4e4f92f2d8dc5c`

### Implemented

Added mandatory Phase -1 before Phase 0 with classifications:

- `ENTRYPOINT`
- `NORMATIVE`
- `SOURCE`
- `PROCEDURE`
- `DERIVED`
- `EVIDENCE`
- `EXPLANATORY`

Required artifacts:

```text
CONTEXT INVENTORY
CONTEXT SUMMARY
Ready for Phase 0: YES/NO
```

Material context gaps now block planning.

### Why

Context loading had been recommended but not enforceable. This made context engineering a normative workflow phase.

### Behavior impact

- no planning before Phase -1 passes
- no code changes during Phase -1
- missing correctness-relevant context blocks progress
- progressive disclosure is mandatory but cannot omit dependencies

---

## Stage 14 — Skill Invocation Contract

**Status:** Done

**Normative contract commit:** `a40a469827e109e1d8a85c7638333ce4a30ea295`

**Skill registry commit:** `89dfdd5a01f6788d2bf1fb1d9ea92725638e582b`

**Context skill commit:** `dd813c17dddd27a53c3197449752205db9392d0d`

**README commit:** `823f4e3c3e83b30b11949d469bb9880ca19a8ddb`

**Changelog synchronization:** this commit

**Files:**

- `AGENTS.md`
- `skills/README.md`
- `skills/context_loading.md`
- `README.md`
- `changelog.md`

### Implemented

Added `PROCEDURE` as a first-class context classification for activated skills.

Defined skill activation conditions:

1. task/phase matches purpose
2. required inputs are available or safely resolvable
3. output is required or materially improves repeatability/traceability/verification
4. no higher authority forbids use

Defined default phase mapping:

- Phase -1 → `skills/context_loading.md`
- Phase 0 through implementation → `skills/rails_api_feature.md` for matching Rails API work
- verification → `skills/quality_gate_review.md` once available, otherwise normative fallback
- post-verification docs → `skills/flow_prd_update.md`

Defined invocation lifecycle:

1. resolve inputs
2. load as `PROCEDURE`
3. verify authority alignment
4. execute phase-relevant procedure
5. produce artifact
6. evaluate completion/failure
7. retain output/decisions rather than unnecessary full skill text

Required artifact:

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

Defined status semantics:

- `ACTIVATED`
- `COMPLETED`
- `BLOCKED`
- `NOT_REQUIRED`
- `UNAVAILABLE`

Defined precedence and conflict rules:

- AGENTS contract layers own process
- requirements own feature behavior
- skills implement procedures only
- conflict marks skill `BLOCKED`
- skill files are not silently edited during a feature task to hide conflict

Defined loading boundaries:

- no bulk skill loading
- multiple active skills require distinct responsibilities
- prefer one owner skill over overlapping procedures
- reference normative sections rather than duplicating them

Defined fallback:

- if a skill is unavailable, execute the normative workflow directly
- record `UNAVAILABLE`
- never waive or weaken a requirement

Updated the registry to show phase, purpose, and availability for each skill.

Updated the context-loading skill to produce its own invocation record and include `PROCEDURE` classification.

Updated README to document activation, lifecycle, invocation status, fallback, and phase usage.

### Why

Before this stage, skills existed but their runtime relationship to the contract was informal. An agent could bulk-load skills, treat them as competing authorities, or skip a phase when a skill was missing.

This stage makes skill use deterministic and reviewable while preserving a direct contract fallback.

### Behavior impact

Normative workflow change:

- activated skills require invocation records
- skills are loaded phase-by-phase
- skill conflicts block the procedure
- missing skills are explicitly reported but never weaken the contract
- skill outputs, not full unrelated skill bodies, carry forward between phases

### Decisions

- Skills remain optional implementations of mandatory behavior unless explicitly mapped as the normal procedure.
- The context-loading skill is the normal Phase -1 procedure, but Phase -1 itself is contract-owned.
- The future quality-gate skill is already mapped with a safe normative fallback.
- Multi-skill use is allowed only for distinct, composable responsibilities.

### Next stage

Stage 15: implement `skills/quality_gate_review.md` so verification and contract-audit evidence follow the same skill model.

---

# 6. Remaining Stages

## Stage 15 — Quality Gate Review Skill

**Status:** Next

Planned responsibilities:

- contract-alignment evidence
- exact verification command selection
- command and exit-code reporting
- failure summary
- discrepancy classification
- readiness decision for Flow/PRD/changelog updates
- skill invocation and completion record

## Stage 16 — Skill Consistency Audit

**Status:** Planned

- normalize skill structure
- reduce normative duplication
- verify terminology, inputs, outputs, completion, and failure modes
- introduce `templates/SKILL_TEMPLATE.md` if justified

## Stage 17 — Automated Skills and Docs Audit

**Status:** Planned

Potential checks:

- required skill headings
- authority declarations
- internal path/link integrity
- README index completeness
- missing referenced files
- duplicate skill ownership

## Stage 18 — Final Integration Audit

**Status:** Planned

- README/docs/skills/AGENTS consistency
- no duplicate authorities
- no contradictory workflow stages
- explicit context and skill loading
- mandatory verification
- scripts/docs alignment
- synchronized changelog
- final PR description

---

# 7. Current Repository State After Stage 14

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

The repository now has a thin entrypoint, preserved full contract, normative context loading, deterministic skill activation, explicit invocation evidence, reusable procedures, documented quality gates, and a stage-governed migration path.
