# Agentic Workflow Alignment Changelog

This document records how the repository evolved from a strict Rails API contract toolkit into an explicitly agent-ready engineering workflow, why each change was introduced, which external and internal resources influenced the direction, what has already been implemented in PR #3, and what remains planned inside the same pull request.

## Change Scope

- Repository: `lukin-io/agents`
- Branch: `chore/agentic_update`
- Pull request: `#3 - Clarify agent-ready workflow positioning`
- Base branch: `main`
- Work period covered: May 10, 2026 through July 23, 2026
- Change type: positioning, documentation alignment, skill extraction, context-engineering preparation, and workflow behavior migration planning

The pull request is intentionally used as an umbrella change. Each logical stage is committed separately so the evolution can be reviewed, discussed, adjusted, or reverted stage by stage.

---

## 1. Starting Point

The repository originally contained a compact Rails API enforcement toolkit:

```text
AGENTS.md
verify
contract_audit
templates/
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

Its practical purpose was already strong:

- implement features against `doc/requirements/**`
- keep API behavior contract-first
- enforce canonical response envelopes
- keep Blueprinter responsible for `data` payloads
- use Pundit, Ransack, and Kaminari consistently
- require a planning-first implementation process
- run repeatable verification before merge
- detect contract and documentation drift
- maintain structured Flow and PRD documents
- produce traceable implementation evidence

However, the repository described itself primarily as Rails API contract and enforcement tooling. It did not yet explicitly explain how the same mechanics map to modern agent engineering, context engineering, reusable skills, verification-driven development, or AI-assisted software delivery.

The main problem was therefore not that the workflow was missing. The problem was that the existing workflow had not yet been named, structured, and documented using the emerging agent-engineering vocabulary that best described what it was already doing.

---

## 2. Research and Discussion Path

The direction was derived through three linked research stages.

### Stage 1: Industry Direction — What Is Changing

The first resource group was used to understand the broader shift from prompt-centric AI use toward reusable execution systems.

Resources:

- https://youtu.be/igO8iyca2_g?t=1
- https://youtu.be/_R83pFpUWyM?t=1
- https://youtu.be/5ID22ACI7IM?t=1
- https://x.com/garrytan/status/2053127519872614419?s=20

Concepts extracted from this stage:

- AI is moving from single-response chat toward multi-step execution.
- One-off prompts do not compound well across teams or repositories.
- Reusable workflows and skills are more valuable than repeatedly authored large prompts.
- A useful architecture is “fat skills, thin harness”: strong reusable procedures with a small orchestration layer.
- Verification becomes more important as generation becomes cheaper.
- Durable advantage comes from workflow, context, knowledge, and verification infrastructure rather than from selecting one model.

This stage answered:

```text
What is happening in AI-assisted engineering?
```

### Stage 2: Implementation Platforms — How It Is Built

The second resource group was used to connect the industry direction to concrete agent-building primitives.

Resources:

- https://openai.com/solutions/use-case/agents/
- https://developers.openai.com/api/docs/guides/agent-builder
- https://developers.openai.com/api/docs/guides/agents
- https://openai.com/uk-UA/agent-platform/

Concepts extracted from this stage:

- an agent is an execution loop, not only an LLM response
- tools, state, handoffs, approvals, traces, and evaluations are first-class parts of agent systems
- visual workflow builders and code-first SDKs are two interfaces over similar orchestration concerns
- production systems require observability and evaluation, not only generation
- multi-agent designs should be introduced only where role separation is useful
- reliable single-agent or single-workflow systems are often preferable to premature multi-agent complexity

This stage answered:

```text
How are agent systems assembled and operated?
```

### Stage 3: Reliability — How To Make It Work Consistently

The third resource group focused on context engineering and reusable agent skills.

Resources:

- https://x.com/Kappaemme1926/status/2050908233158816122
- https://www.youtube.com/watch?v=esY99nYXxR4

Concepts extracted from this stage:

- context quality matters more than simply increasing context volume
- context windows are capacity, not guaranteed understanding
- agents need deliberate context loading and progressive disclosure
- a tool is a capability; a skill is a repeatable procedure for using capabilities correctly
- large universal prompts accumulate contradictions and context noise
- responsibilities should be decomposed into reusable procedures with clear inputs, outputs, checks, and failure modes
- verification, traceability, and stable context structure are central to reliable execution

This stage answered:

```text
Why do agent workflows drift, and how should they be made reliable?
```

---

## 3. Internal Evidence and Existing Practice

The research was not adopted as an abstract replacement for the existing workflow. It was compared against the engineering system already in use.

### Existing Repository Assets

#### `AGENTS.md`

The existing file already acted as a unified execution contract by defining:

- document authority and precedence
- allowed and forbidden edit scopes
- requirement ownership
- no-code planning phases
- implementation confirmation gates
- Rails conventions
- response and error invariants
- verification order
- contract compliance review
- Flow and PRD rules
- final output evidence

This mapped naturally to the term:

```text
Agent Operating Contract
```

#### `verify`

The verification runner already implemented reusable quality profiles:

- fast verification for normal implementation work
- full verification for broader schema, seed, security, and process-sensitive work
- RuboCop
- RSpec
- conditional RSwag generation
- Brakeman
- Bundle Audit
- seed verification
- parallel test database preparation
- changed-surface detection

This mapped naturally to:

```text
Verification-Driven Development
Quality Gate Runner
Pre-Merge Verification Pipeline
```

#### `contract_audit`

The audit script already checked for:

- requirement document edits
- generated Swagger drift
- database-specific SQL
- route representation drift
- ad-hoc controller JSON
- Flow and PRD template drift

This mapped naturally to:

```text
Contract Drift Audit
Static Contract Guardrail
Implementation Compliance Audit
```

#### Flow and PRD Templates

The templates already provided stable schemas for derived documentation.

This mapped naturally to:

```text
Structured Context Docs
Context Schemas
Documentation as an Interface
```

### Supporting Author Experience

The repository direction was also compared against the author’s published implementation experience:

- https://lukin.io/blog/mergeable-by-default-context-engine-ai-codegen/
- https://lukin.io/blog/1-backend-engineer-vs-11-engineers-with-ai/

Relevant internal principles:

- make generated work mergeable by default
- reduce ambiguity before generation
- encode repository conventions explicitly
- require deterministic verification loops
- treat context as engineering infrastructure
- preserve requirement-to-code traceability
- use automation to increase delivery speed without weakening quality gates

The conclusion was that the repository was already implementing a substantial portion of modern agent-engineering practice, but its public structure and vocabulary did not yet make that clear.

---

## 4. Target Architecture

The target architecture became:

```text
AGENTS.md
  -> normative authority and operating harness

skills/**
  -> reusable execution procedures

docs/**
  -> concepts, adoption guidance, workflow explanation, quality gates, migration notes

templates/**
  -> structured context schemas

verify
  -> verification runner copied to bin/verify in consumer repositories

contract_audit
  -> drift guard copied to bin/contract_audit in consumer repositories
```

The intended execution loop is:

```text
requirement handoff
  -> context loading
  -> contract extraction
  -> repository scan
  -> implementation plan
  -> human confirmation gate
  -> implementation
  -> contract alignment check
  -> verification
  -> contract audit
  -> derived documentation
  -> final evidence report
```

The architectural principle is:

```text
fat skills, thin harness
```

In this repository:

- `AGENTS.md` is the harness and authority model
- `skills/**` contains reusable operational procedures
- `verify` and `contract_audit` enforce completion gates
- `docs/**` explains the system without becoming normative authority
- `templates/**` keeps derived context predictable

---

## 5. Implemented Changes in PR #3

### 5.1 `README.md` — Agent-Ready Positioning

Status: implemented.

The README was changed from a narrow Rails enforcement description to:

```text
Agent-ready contract-first workflow toolkit for Rails API teams.
```

Implemented changes:

- introduced the term **Agent Operating Contract**
- documented the full execution chain from requirements to final report
- explained that the toolkit supports both human engineers and coding agents
- added context loading and planning gates to the stated purpose
- documented the problems addressed:
  - prompt drift
  - context pollution
  - contract drift
  - verification gaps
  - unreviewable AI output
- introduced preferred terminology:
  - Agent Operating Contract
  - Contract-First Execution
  - Context Engineering
  - Verification-Driven Development
  - Contract Drift Audit
  - Requirement-to-Code Traceability
  - Structured Context Docs
  - AI-Assisted SDLC
  - Agent-Ready Engineering Workflow

Why this was necessary:

The implementation already behaved as an agent-ready workflow, but external readers could interpret it as only a Rails style guide plus two scripts. The README now communicates the larger system model.

### 5.2 `docs/CONCEPTS.md` — Shared Vocabulary

Status: implemented.

Purpose:

Create a stable glossary so terms are not redefined differently in the README, `AGENTS.md`, skills, PR descriptions, CV text, or future blog posts.

Implemented definitions:

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

The file also explains what the toolkit is not:

- not a chatbot prompt pack
- not a general-purpose autonomous-agent runtime
- not a no-code automation product
- not a replacement for Rails conventions

Why this was necessary:

Without explicit definitions, terms such as “agent”, “skill”, “context”, and “verification” can become marketing language. This file binds those terms to concrete repository artifacts and behaviors.

### 5.3 `docs/WORKFLOW.md` — Adoption and Execution Map

Status: implemented.

Purpose:

Provide a shorter operational map for engineers who should not need to read the entire normative contract before understanding the system.

Documented workflow:

1. requirement handoff
2. context extraction
3. repository scan
4. implementation plan
5. confirmation gate
6. implementation
7. contract alignment check
8. verification
9. contract audit
10. derived docs and changelog
11. final report

Additional implementation detail:

- defines the human/agent responsibility split
- keeps humans responsible for product intent, requirement approval, architecture tradeoffs, confirmation, and merge decisions
- identifies extraction, scanning, planning, implementation, verification evidence, and documentation updates as procedures suitable for coding agents
- explains why the process exists and which failure modes it reduces

Why this was necessary:

`AGENTS.md` is deliberately comprehensive and normative. A separate workflow guide improves onboarding and understanding without weakening or duplicating authority.

### 5.4 `docs/QUALITY_GATES.md` — Verification Model

Status: implemented.

Purpose:

Explain the existing quality gates as one verification system rather than as unrelated commands.

Documented gate sequence:

```text
contract alignment check
  -> bin/verify
  -> bin/contract_audit --all
  -> derived docs and changelog
  -> final evidence report
```

Implemented documentation:

- pre-command contract alignment checklist
- fast verification profile purpose
- full verification profile purpose
- contract audit scope
- requirement handoff exception
- exact final `CHECKS` reporting expectations
- rationale for preventing AI-assisted implementation drift

Why this was necessary:

The scripts already existed, but reviewers and new adopters needed one place explaining why both are required, what each protects, and when documentation may be updated.

### 5.5 `docs/WORKFLOW_MIGRATION.md` — Controlled Migration Plan

Status: implemented.

Purpose:

Record the staged path from the current unified contract toward a clearer agent-ready system without performing an uncontrolled rewrite of `AGENTS.md`.

Documented migration stages:

- positioning
- quality gates
- skill registry
- `AGENTS.md` linking
- stricter context loading
- skill invocation rules
- quality gate skill
- README index completion

The document also explicitly states which rules should not change yet:

- response envelope rules
- status-code rules
- documentation template rules
- verification command order
- requirement read-only policy
- Flow/PRD version ownership
- final report requirements

Why this was necessary:

The workflow should evolve incrementally. The migration document separates already-strong invariants from areas that logically need behavior changes.

### 5.6 `skills/README.md` — Initial Skill Registry

Status: implemented.

Purpose:

Create an explicit reusable procedure layer around the existing operating contract.

The file defines a skill as more than a prompt. A skill includes:

- usage conditions
- required inputs
- execution steps
- expected outputs
- verification gates
- failure modes
- handoff notes

It establishes the authority relationship:

```text
AGENTS.md remains normative.
skills/** helps apply AGENTS.md.
A skill cannot override AGENTS.md.
```

Why this was necessary:

The existing contract contained multiple reusable procedures inside one large file. Extracting procedures into skills supports modular loading, reduces repeated task instructions, and prepares the repository for progressive context disclosure.

### 5.7 `skills/context_loading.md` — Context Engineering Procedure

Status: implemented.

Purpose:

Make context loading an explicit reusable operation before planning or implementation.

Implemented procedure:

- read relevant normative sections
- read the target requirement document and all versions
- identify requirement-owned version authority
- locate mirrored Flow/PRD docs
- classify Flow/PRD as derived context
- scan routes, controllers, models, blueprints, policies, services, specs, migrations, and seeds
- produce a compact context summary
- identify context gaps before implementation

Expected output includes:

```text
CONTEXT SUMMARY
- Requirement source
- Latest requirement version
- Derived Flow doc
- Derived PRD doc
- Existing implementation surfaces
- Known related specs
- Context gaps
```

Why this was necessary:

The original Phase 0 and Phase 1 behavior already performed much of this work, but the process was not isolated as a reusable skill. The new file makes context engineering visible and independently loadable.

### 5.8 `skills/rails_api_feature.md` — Feature Implementation Procedure

Status: implemented.

Purpose:

Extract the standard feature implementation path into a reusable procedure.

Implemented phases:

- contract extraction
- repository scan
- file-by-file planning
- confirmation gate
- implementation
- verification
- derived documentation
- final evidence output

The skill preserves core rules:

- no code before the planning gate
- Blueprinter owns `data`
- controllers own envelopes
- Pundit owns authorization
- Ransack owns filtering where applicable
- Kaminari owns pagination where applicable
- verification and contract audit are mandatory
- Flow and PRD documents remain derived

Why this was necessary:

This is the main “fat skill” corresponding to the existing feature workflow. It can be loaded for implementation tasks without requiring every documentation-specific detail to occupy the active context immediately.

### 5.9 `skills/flow_prd_update.md` — Derived Documentation Procedure

Status: implemented.

Purpose:

Separate post-verification documentation work from feature implementation work.

Implemented rules:

- update docs after verification
- mirror requirement paths
- reference requirement-owned versions only
- maintain integration-facing Flow content
- maintain product/scope-focused PRD content
- include Drift Delta
- maintain traceability to tasks, endpoints, implementation files, and specs

Why this was necessary:

Documentation updates are a distinct procedure with different inputs and outputs. Extracting them reduces context load during coding and clarifies that Flow/PRD docs are generated from verified implementation rather than used to override requirements.

---

## 6. Commit and Safety History

The work was intentionally split into multiple commits.

Known logical commit sequence:

1. `Clarify agent-ready workflow positioning`
2. `Add agentic workflow concepts`
3. `Add agentic workflow map`
4. attempted small `AGENTS.md` orientation update
5. `Restore AGENTS contract after partial connector update`
6. `Document verification quality gates`
7. `Add skill registry overview`
8. `Add context loading skill`
9. `Add Rails API feature skill`
10. `Add Flow and PRD update skill`
11. `Add workflow migration notes`
12. `Document agentic workflow evolution` — this changelog

### `AGENTS.md` Recovery

A small introductory `AGENTS.md` change was attempted through the GitHub connector.

The connector returned a truncated representation of the large file, and a full-file replacement would have removed a significant part of the contract. The issue was detected by comparing the branch to `main` before merge.

Recovery action:

- restore `AGENTS.md` from `main`
- commit the restoration
- verify that `AGENTS.md` disappeared from the net PR diff

Current result:

- no normative `AGENTS.md` behavior change is present in the current PR diff
- the full intended migration is documented before being applied
- future `AGENTS.md` changes should be made locally as small patches or through a tool that supports safe partial edits

This recovery is important evidence of the workflow itself: verification of the diff prevented an unintended destructive documentation change.

### Connector Write Constraints

Some later connector writes were blocked while attempting:

- README index consolidation
- a dedicated `skills/quality_gate_review.md` file

No partially written or broken files were committed from those attempts.

Equivalent concepts remain documented in:

- `docs/QUALITY_GATES.md`
- `docs/WORKFLOW_MIGRATION.md`
- `skills/README.md`

The blocked items remain explicit follow-up tasks in the same PR.

---

## 7. Current PR State at Changelog Creation

Current changed files before adding this changelog:

```text
README.md

docs/CONCEPTS.md
docs/QUALITY_GATES.md
docs/WORKFLOW.md
docs/WORKFLOW_MIGRATION.md

skills/README.md
skills/context_loading.md
skills/flow_prd_update.md
skills/rails_api_feature.md
```

Current characteristics:

- the PR is mergeable
- the branch is `chore/agentic_update`
- `AGENTS.md` is unchanged in the net diff
- runtime verification scripts are unchanged
- contract audit behavior is unchanged
- templates are unchanged
- current work is documentation, architecture definition, and initial skill extraction

---

## 8. What Has Been Achieved

Before this PR, the repository was accurately described as:

```text
Rails API contract and enforcement tooling.
```

After the implemented stages, it can be more precisely described as:

```text
An agent-ready, contract-first Rails API workflow toolkit with a normative operating contract, structured context docs, reusable execution skills, verification profiles, contract drift guards, and requirement-to-code traceability.
```

The key change is not the addition of AI branding.

The key change is that existing engineering mechanics are now represented as a coherent system:

- source-of-truth context
- derived context
- planning gates
- reusable skills
- implementation rules
- verification gates
- drift audits
- final evidence

---

## 9. Pending Work Inside PR #3

The following logical stages remain planned before the umbrella PR reaches its final state.

### 9.1 README Index Completion

Update README to include:

- `docs/QUALITY_GATES.md`
- `docs/WORKFLOW_MIGRATION.md`
- `skills/**`
- Skill Registry terminology
- complete repository tree

### 9.2 Safe `AGENTS.md` Orientation

Add a small non-behavioral introduction:

- identify `AGENTS.md` as the Agent Operating Contract
- link to concepts, workflow, quality gates, and skills
- state clearly that supporting docs are non-normative

### 9.3 Explicit Context Loading Behavior

Potential normative change:

- require a list of loaded context before Phase 0 output
- classify each source as normative, source-of-truth, derived, or implementation evidence
- require a compact context summary
- require known context gaps to be declared before planning

### 9.4 Skill Invocation Rules

Potential normative change:

- define when skills should be loaded
- state that skills are execution aids, not independent authorities
- require conflict resolution in favor of `AGENTS.md`
- avoid loading all skills for every task

### 9.5 Quality Gate Review Skill

Add:

```text
skills/quality_gate_review.md
```

Expected purpose:

- collect exact verification commands and exit codes
- prepare `CHECKS`
- prepare rule compliance evidence
- block derived docs until gates pass

### 9.6 Workflow Behavior Review

Review whether the current hard confirmation gate should remain universal or become task-mode specific.

The current gate is strong for high-risk feature work, but future refinement may distinguish:

- planning-only tasks
- implementation tasks requiring approval
- explicitly autonomous implementation tasks
- documentation-only tasks

Any change should preserve auditability and must not silently weaken the default contract.

---

## 10. Decision Principles For Remaining Work

All remaining changes should follow these rules:

1. Preserve source-of-truth authority.
2. Keep `AGENTS.md` as the only normative workflow contract.
3. Treat skills as reusable procedures, not competing policy documents.
4. Load only relevant context.
5. Keep generation separate from verification.
6. Require evidence before derived documentation is updated.
7. Prefer small reviewable commits.
8. Use repository diffs to detect accidental contract loss.
9. Avoid multi-agent or orchestration complexity without a concrete need.
10. Prefer reliable Rails conventions over generic agent abstractions.

---

## 11. Summary

This change began as a terminology and profile-alignment exercise: identify how existing work related to agents, context engineering, contract validation, and verification-driven development.

It evolved into a repository architecture update because the comparison showed that the existing system already contained the core mechanics of an agent-ready workflow.

The implemented result currently provides:

- clearer public positioning
- a shared conceptual model
- a readable workflow map
- quality gate documentation
- a controlled migration plan
- an initial skill registry
- a context loading skill
- a Rails API implementation skill
- a Flow/PRD update skill
- this detailed evolution changelog

The next stages will move from documentation alignment into explicit workflow behavior migration while preserving the strict contract and verification mechanisms that made the original toolkit effective.
