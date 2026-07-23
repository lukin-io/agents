# Workflow Migration Notes

This document records the completed migration from the original unified Rails API contract toolkit to an agent-ready workflow system.

The migration preserved existing contract discipline, made context and phase ownership explicit, extracted reusable procedures, and added mechanical enforcement after the structures stabilized.

## Final Architecture

```text
AGENTS.md
  -> small normative entrypoint
  -> Phase -1 context loading
  -> skill invocation
  -> conditional agentic audit gate

AGENTS_CONTRACT.md
  -> preserved complete Rails API contract
  -> Phase 0 onward, engineering rules, verification, docs, final evidence

skills/**
  -> phase-owned reusable procedures
  -> canonical schema, outputs, completion, failure, and handoffs

templates/SKILL_TEMPLATE.md
  -> source skill schema
  -> installed as doc/templates/SKILL_TEMPLATE.md

verify
  -> implementation/test/security/docs verification profiles

contract_audit
  -> Rails API and documentation contract drift checks

agentic_audit
  -> toolkit/consumer skill, registry, reference, template, and contract-link checks

.github/workflows/agentic-audit.yml
  -> Ruby syntax, toolkit audit, and consumer-layout audit evidence
```

## Final Execution Chain

```text
CONTEXT INVENTORY + CONTEXT SUMMARY
  -> FEATURE IMPLEMENTATION HANDOFF
  -> CHECKS + compliance + traceability
  -> QUALITY GATE DECISION
  -> DOC UPDATE HANDOFF when required
  -> final evidence report
```

Primary skill sequence:

```text
context_loading
  -> rails_api_feature
  -> quality_gate_review
  -> flow_prd_update when required
  -> final reporting
```

## Preserved Principles

- requirements remain feature/API source of truth
- Flow/PRD docs remain derived
- no-code planning and confirmation gates remain
- Blueprinter/Pundit/Ransack/Kaminari rules remain
- verification and API contract audit remain mandatory
- requirement-owned versioning remains
- final evidence remains mandatory
- skills cannot override either AGENTS contract layer
- no command success is inferred without evidence

## Completed Migration Stages

### A — Positioning and Vocabulary

**Done**

- agent-ready README positioning
- Agent Operating Contract vocabulary
- concepts and workflow guides

### B — Quality-Gate Model

**Done**

- verification/audit sequence documentation
- exact command evidence
- `QUALITY GATE DECISION: PASS/BLOCKED`

### C — Skill Registry

**Done**

- context loading
- Rails API implementation
- quality gate review
- Flow/PRD updates

### D — AGENTS Orientation Split

**Done**

- small root `AGENTS.md`
- original full contract preserved as `AGENTS_CONTRACT.md`
- authority/load-order boundaries

### E — Normative Context Loading

**Done**

- Phase -1
- context classifications
- inventory/summary
- dependency-driven expansion
- material-gap stop gate

### F — Skill Invocation Contract

**Done**

- activation, precedence, lifecycle, statuses, conflicts, and fallback
- phase-by-phase loading
- invocation evidence

### G — Quality Gate Skill

**Done**

- profile selection
- command evidence
- compliance and traceability review
- documentation-readiness decision

### H — README Consolidation

**Done**

- complete architecture/index/installation entrypoint

### I — Skill Consistency Audit

**Done**

- canonical skill template
- one primary owner per phase
- stable outputs and handoffs
- reduced overlap/duplication

### J — Automated Skills and Docs Audit

**Done**

- toolkit/consumer audit scopes
- required-file/schema/title/registry/ownership/index/reference/template/link checks
- conditional normative gate
- quality-skill integration
- executable script
- GitHub Actions workflow

### K — Final Integration Audit

**Done**

Final review covered:

- complete branch comparison against `main`
- all 17 changed files
- AGENTS two-layer authority consistency
- Phase -1 and skill invocation consistency
- skill schema and ownership consistency
- README/docs/skills/templates/tooling discoverability
- conditional audit semantics
- toolkit and simulated consumer execution
- stale concept/workflow wording cleanup
- PR mergeability
- unresolved review threads and submitted reviews

Findings resolved during final integration:

- `docs/CONCEPTS.md` was updated from the old single-file AGENTS model to the final two-layer contract, Phase -1, skills, quality decision, and agentic audit.
- CI was expanded from toolkit-only execution to a simulated consumer installation layout.
- `docs/AGENTIC_AUDIT.md` was updated with CI steps, evidence, and scope boundaries.

## Final Automated Evidence

Successful workflow before final closure docs:

```text
Workflow: Agentic workflow audit
Run ID: 30013399571
Commit: eb0b62f5bbd1f28be70a78a0479aa327f8a516cd
Conclusion: success
```

Successful required step model:

```text
Check Ruby syntax
Run toolkit audit
Build consumer installation layout
Run consumer-layout audit
```

Because this migration/changelog closure itself changes audited docs, GitHub Actions runs again on the final head. The authoritative final-head result is the PR check, not a self-referential run number committed into the file that triggers the next run.

## Final Scope Boundaries

The completed system verifies workflow structure and execution evidence. It does not replace:

- product/requirement approval
- semantic code review
- Rails tests and security checks
- API contract review
- human merge approval

The PR is left open and mergeable for owner review. It is not merged automatically.
