# Agentic Workflow Alignment Changelog

This document records how `lukin-io/agents` is evolving from a strict Rails API contract toolkit into an explicitly agent-ready engineering workflow. It explains the research path, the reasoning behind the architecture, the implemented stages in PR #3, the behavior impact of each stage, and the remaining migration work.

## Change Scope

- Repository: `lukin-io/agents`
- Branch: `chore/agentic_update`
- Pull request: `#3`
- Base branch: `main`
- Work period: May 10, 2026 onward
- Current update date: July 23, 2026
- Change type: positioning, documentation alignment, skill extraction, stricter context engineering, verification-driven workflow migration, and enforcement preparation

PR #3 is intentionally an umbrella pull request. Each logical stage is committed separately so it can be reviewed, discussed, adjusted, or reverted independently.

## Stage Completion Rule

A stage is considered complete only after all of the following happen:

1. The stage implementation is committed and pushed to `chore/agentic_update`.
2. This changelog is updated with:
   - status
   - files changed
   - what changed
   - why it changed
   - workflow or behavior impact
   - limitations or decisions
   - next stage
3. The changelog update is committed and pushed separately.
4. A concise stage summary is provided to the project owner.

This rule applies to all remaining stages in PR #3.

---

# 1. Starting Point

The repository originally contained:

```text
AGENTS.md
verify
contract_audit
templates/
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

The original system already provided substantial workflow discipline:

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
- stable Flow/PRD document schemas
- requirement-to-code traceability
- final evidence reporting

The main gap was not the absence of a workflow. The workflow already existed and was strong. The gap was that its public positioning and structure did not explicitly describe how it mapped to modern agent engineering, context engineering, reusable skills, verification-driven development, or AI-assisted software delivery.

The repository could be misunderstood as a Rails style guide plus two scripts, despite already behaving like a repository-level execution and verification system.

---

# 2. Research Path

The migration direction came from three linked research stages.

## 2.1 Industry Direction — What Is Changing

Resources:

- https://youtu.be/igO8iyca2_g?t=1
- https://youtu.be/_R83pFpUWyM?t=1
- https://youtu.be/5ID22ACI7IM?t=1
- https://x.com/garrytan/status/2053127519872614419?s=20

Important conclusions:

- AI-assisted work is moving from single-response chat toward repeatable execution systems.
- One-off prompts do not compound reliably across teams and repositories.
- Reusable workflows and skills are more valuable than repeatedly authored large prompts.
- A useful architecture is **fat skills, thin harness**:
  - strong reusable operational procedures
  - small authority/orchestration layer
- Generation becomes cheaper; verification becomes more strategically important.
- Durable value comes from context, workflow, knowledge, verification, and traceability—not only model choice.

This research stage answered:

```text
What is changing in AI-assisted engineering?
```

## 2.2 Agent Platforms — How It Is Built

Resources:

- https://openai.com/solutions/use-case/agents/
- https://developers.openai.com/api/docs/guides/agent-builder
- https://developers.openai.com/api/docs/guides/agents
- https://openai.com/uk-UA/agent-platform/

Important conclusions:

- An agent is an execution loop, not only a generated response.
- Tools, state, handoffs, approval gates, traces, and evaluations are system components.
- Visual builders and code-first SDKs expose similar orchestration concerns through different interfaces.
- Production workflows require observability and evaluation.
- Multi-agent architecture should not be introduced without a useful responsibility boundary.
- A reliable single workflow is often better than premature orchestration complexity.

This stage answered:

```text
How are agent systems assembled and operated?
```

## 2.3 Context and Reliability — How It Works Consistently

Resources:

- https://x.com/Kappaemme1926/status/2050908233158816122
- https://www.youtube.com/watch?v=esY99nYXxR4

Important conclusions:

- Context quality matters more than context volume.
- A large context window is capacity, not guaranteed understanding.
- Context should be loaded deliberately and progressively.
- A tool is a capability; a skill is a procedure for using capabilities correctly.
- Large universal prompts accumulate contradictions and noise.
- Reusable procedures should define inputs, steps, outputs, checks, and failure modes.
- Verification and traceability are essential to reliable execution.

This stage answered:

```text
Why do agent workflows drift, and how should they be made reliable?
```

## 2.4 Internal Evidence

The external direction was compared against existing practical work and published experience:

- https://lukin.io/blog/mergeable-by-default-context-engine-ai-codegen/
- https://lukin.io/blog/1-backend-engineer-vs-11-engineers-with-ai/

Relevant principles already demonstrated internally:

- generated work should be mergeable by default
- ambiguity should be reduced before implementation
- repository conventions should be explicit
- verification should be deterministic
- context should be treated as engineering infrastructure
- requirements, implementation, specs, and docs should remain traceable
- delivery speed should increase without weakening quality gates

The conclusion was that the repository already implemented much of the desired engineering discipline. The migration should therefore expose, modularize, and strengthen the system—not replace it with agent terminology or unnecessary orchestration.

---

# 3. Target Architecture

The target architecture is:

```text
AGENTS.md
  -> normative Agent Operating Contract
  -> authority, workflow gates, engineering invariants, final reporting

skills/**
  -> reusable execution procedures
  -> progressively loaded when relevant

verify
  -> source for bin/verify in consumer repositories
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
  -> research path, decisions, stage status, behavior impact, remaining work
```

The execution loop is:

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

The governing architectural principle is:

```text
fat skills, thin harness
```

`AGENTS.md` remains the thin authority and execution harness. Skills contain reusable procedures. Verification scripts provide deterministic completion gates.

---

# 4. Completed Stages

## Stage 1 — Agent-Ready Positioning

**Status:** Done

**Primary file:** `README.md`

### What changed

- Reframed the repository as an agent-ready, contract-first workflow toolkit.
- Introduced **Agent Operating Contract** terminology.
- Documented the end-to-end execution chain.
- Explicitly stated support for both human engineers and coding agents.
- Added the problems addressed:
  - prompt drift
  - context pollution
  - contract drift
  - verification gaps
  - unreviewable generated output

### Why

The repository already implemented an agent-ready workflow but looked narrower than it was. Correct positioning makes the system understandable to engineers, hiring audiences, potential adopters, and future contributors.

### Behavior impact

Documentation and positioning only. No normative workflow behavior changed.

---

## Stage 2 — Concepts and Shared Vocabulary

**Status:** Done

**File:** `docs/CONCEPTS.md`

### What changed

Defined concrete meanings for:

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

Also documented what the toolkit is not:

- not a chatbot prompt pack
- not a general autonomous-agent runtime
- not a no-code system
- not a replacement for Rails conventions

### Why

Terms such as “agent”, “skill”, “context”, and “verification” become weak marketing terms unless mapped to concrete files, authority, outputs, and gates.

### Behavior impact

Explanatory only. Provides stable vocabulary for subsequent workflow changes.

---

## Stage 3 — Workflow Adoption Guide

**Status:** Done

**File:** `docs/WORKFLOW.md`

### What changed

Documented the compact execution map:

1. requirement handoff
2. context extraction
3. repository scan
4. implementation plan
5. confirmation gate
6. implementation
7. contract alignment
8. verification
9. contract audit
10. derived docs and changelog
11. final report

Also documented human/tool responsibility boundaries.

### Why

`AGENTS.md` is intentionally comprehensive and normative. A shorter adoption guide lets engineers understand the system without creating a second policy authority.

### Behavior impact

Explanatory only.

---

## Stage 4 — Quality Gates Documentation

**Status:** Done

**File:** `docs/QUALITY_GATES.md`

### What changed

Unified existing commands into one gate model:

```text
contract alignment
  -> bin/verify
  -> bin/contract_audit --all
  -> derived docs/changelog
  -> final evidence
```

Documented:

- pre-verification alignment checks
- fast profile intent
- full profile intent
- contract audit scope
- requirement handoff exception
- exact `CHECKS` evidence expectations

### Why

The scripts existed, but adopters needed a single explanation of why both are required, what they protect, and when documentation updates become allowed.

### Behavior impact

Documentation only. Existing command order remains unchanged.

---

## Stage 5 — Initial Skill Registry

**Status:** Done

**File:** `skills/README.md`

### What changed

Introduced a formal skill model. A skill defines:

- activation conditions
- required inputs
- execution steps
- expected outputs
- checks
- failure modes
- handoff notes

Defined authority:

```text
AGENTS.md remains normative.
skills/** applies AGENTS.md.
A skill cannot override AGENTS.md or requirement contracts.
```

### Why

The original unified contract included multiple reusable procedures. Extracting them supports modular context loading and reduces repeated instructions.

### Behavior impact

Introduces a reusable procedure layer, but does not yet change normative skill invocation rules in `AGENTS.md`.

---

## Stage 6 — Context Loading Skill

**Status:** Done

**File:** `skills/context_loading.md`

### What changed

Created an explicit procedure to:

- load relevant normative sections
- read the target requirement and all versions
- identify requirement-owned version authority
- locate mirrored Flow/PRD docs
- classify Flow/PRD as derived context
- scan implementation and spec surfaces
- report context gaps
- produce a compact `CONTEXT SUMMARY`

### Why

Context engineering must be an explicit procedure rather than an implicit instruction to “read the repo.”

### Behavior impact

Provides a reusable procedure. Normative enforcement in `AGENTS.md` remains a later stage.

---

## Stage 7 — Rails API Feature Skill

**Status:** Done

**File:** `skills/rails_api_feature.md`

### What changed

Extracted the feature lifecycle into a reusable skill:

- contract extraction
- repository scan
- planning
- confirmation
- implementation
- verification
- derived documentation
- final evidence report

### Why

Feature implementation is the central repeated workflow and should be loadable as a defined capability rather than reconstructed from ad-hoc instructions.

### Behavior impact

Procedure layer only. `AGENTS.md` still owns all normative rules.

---

## Stage 8 — Flow/PRD Update Skill

**Status:** Done

**File:** `skills/flow_prd_update.md`

### What changed

Created a reusable post-verification procedure for:

- mirrored requirement/Flow/PRD paths
- requirement-owned version labels
- integration-facing Flow contents
- product-facing PRD contents
- traceability
- migration impact
- Drift Delta

### Why

Derived documentation has different authority and timing from implementation. It should be a separate skill loaded only after verification gates pass.

### Behavior impact

Procedure layer only.

---

## Stage 9 — Controlled Migration Plan

**Status:** Done

**File:** `docs/WORKFLOW_MIGRATION.md`

### What changed

Documented the safe migration path:

- positioning
- quality-gate documentation
- skill extraction
- `AGENTS.md` orientation
- stricter context loading
- skill invocation rules
- quality-gate skill
- README completion
- later automated enforcement

Also documented invariants that should not change casually:

- envelope rules
- status-code rules
- requirement read-only policy
- verification order
- Flow/PRD version ownership
- final report evidence

### Why

The workflow should evolve through controlled behavior changes, not a large rewrite of the normative contract.

### Behavior impact

Migration planning only.

---

## Stage 10 — Historical and Decision Changelog

**Status:** Done

**File:** `changelog.md`

### What changed

Created the durable record of:

- original repository state
- external resources
- extracted concepts
- internal evidence
- target architecture
- implemented files
- decisions and limitations
- remaining migration path

### Why

The umbrella PR spans positioning, documentation, skills, and future workflow behavior. Reviewers need to understand how and why the system evolved.

### Behavior impact

Documentation and governance only.

---

## Stage 11 — README Consolidation

**Status:** Done

**Implementation commit:** `8586de6e9978c2c254cdd2513b3367e78cbcbdec`

**Files:**

- `README.md`
- `changelog.md` in the following synchronization commit

### What changed

The README is now the complete repository entrypoint.

It now documents:

- the execution system at a glance
- the **fat skills, thin harness** architecture
- explicit authority precedence
- separation of normative contract, skills, explanatory docs, templates, and tools
- all core docs:
  - `docs/CONCEPTS.md`
  - `docs/WORKFLOW.md`
  - `docs/QUALITY_GATES.md`
  - `docs/WORKFLOW_MIGRATION.md`
  - `changelog.md`
- all current skills:
  - `skills/context_loading.md`
  - `skills/rails_api_feature.md`
  - `skills/flow_prd_update.md`
- progressive skill-loading order
- consumer repository installation layout including skills
- updated execution sequence
- complete repository tree
- preferred terminology
- explicit non-goals

### Why

Prior stages introduced new docs and skills, but the README still indexed only the first documentation files. A user entering through README could not discover the actual architecture. This stage makes the repository self-explanatory and prevents hidden or orphaned workflow artifacts.

### Behavior impact

No normative workflow behavior changed. However, adoption behavior is clearer:

- users are instructed to load context progressively
- skills are explicitly presented as subordinate to `AGENTS.md`
- all architecture layers are discoverable from one entrypoint

### Decisions

- README is an entrypoint, not a second normative policy file.
- `AGENTS.md` remains the only workflow authority.
- Progressive disclosure is documented before it becomes a normative requirement in Stage 13.

### Next stage

Stage 12: add the orientation and authority links directly to `AGENTS.md` without yet changing the core execution behavior.

---

# 5. Remaining Stages

## Stage 12 — `AGENTS.md` Orientation Layer

**Status:** Next

Planned changes:

- explicitly identify `AGENTS.md` as the Agent Operating Contract
- link concepts, workflow, quality gates, migration docs, and skill registry
- define docs as explanatory
- define skills as subordinate execution aids
- preserve all existing normative rules

This is a safe orientation stage, not yet a workflow behavior migration.

## Stage 13 — Stricter Context Loading Behavior

**Status:** Planned

Planned normative changes:

- explicit context-loading step before contract extraction output
- required list of loaded sources
- source classification:
  - normative authority
  - source-of-truth requirements
  - derived context
  - implementation evidence
- required `CONTEXT SUMMARY`
- required context-gap reporting
- progressive disclosure rule
- avoid loading unrelated docs without a discovered dependency

This is a real workflow behavior migration.

## Stage 14 — Skill Invocation Contract

**Status:** Planned

Planned normative changes:

- when skills should be activated
- required skill inputs and outputs
- skill precedence
- no bulk-loading all skills by default
- skills reference normative rules rather than duplicating them
- skill conflict protocol

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

Planned work:

- normalize current skill structure
- remove unnecessary normative duplication
- verify terminology consistency
- verify inputs/outputs/failure modes
- consider `templates/SKILL_TEMPLATE.md`

## Stage 17 — Automated Skills and Docs Audit

**Status:** Planned

Potential `contract_audit` additions:

- required skill headings
- authority declaration checks
- internal path/link checks
- README index completeness
- docs referencing missing files
- invalid or duplicate skill ownership

This stage changes tooling behavior.

## Stage 18 — Final Integration Audit

**Status:** Planned

Final checks:

- README/docs/skills/AGENTS consistency
- no duplicate authorities
- no contradictory workflow stages
- context loading is explicit
- skills are progressively loadable
- verification remains mandatory
- scripts match docs
- changelog is synchronized
- PR description reflects final scope

---

# 6. Current Repository State After Stage 11

```text
AGENTS.md
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

The repository now has:

- clear public positioning
- a single authority model
- explanatory architecture docs
- documented verification gates
- an initial skill registry
- explicit context-loading and implementation procedures
- a migration path toward normative workflow behavior changes
- a stage-synchronized changelog

The next change moves the architecture references into `AGENTS.md` while preserving all existing implementation and verification rules.
