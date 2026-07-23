# Workflow Migration Notes

This document records the migration from the original unified Rails API contract toolkit to an agent-ready workflow system.

Migration principle: preserve existing contract discipline, make context and phase ownership explicit, extract reusable procedures, and add enforcement only after structures stabilize.

## Architecture After Stage 17

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
  -> implementation/test/security verification profiles

contract_audit
  -> Rails API and documentation contract drift checks

agentic_audit
  -> toolkit/consumer skill, docs, registry, reference, and contract-link checks

.github/workflows/agentic-audit.yml
  -> syntax and toolkit audit CI evidence
```

## Migration Principles

- do not weaken the original contract
- keep requirements as feature/API source of truth
- keep Flow/PRD docs derived
- keep verification and contract audit mandatory
- use progressive context disclosure without omitting dependencies
- give each skill one primary phase responsibility
- carry artifacts and decisions between phases
- mechanically enforce structures only after their schema stabilizes
- do not claim command success without executed or inspected evidence

## Completed Stages

### A — Positioning and Vocabulary

**Status:** Done

- agent-ready README positioning
- Agent Operating Contract vocabulary
- concepts and workflow guides

### B — Quality-Gate Model

**Status:** Done

- quality-gate documentation
- exact command evidence
- `QUALITY GATE DECISION: PASS/BLOCKED`

### C — Skill Registry

**Status:** Done

- context loading
- Rails API feature implementation
- quality gate review
- Flow/PRD update

### D — AGENTS Orientation Split

**Status:** Done

- small root entrypoint
- original contract preserved as `AGENTS_CONTRACT.md`
- authority and loading boundaries

### E — Normative Context Loading

**Status:** Done

- Phase -1
- source classifications
- context inventory/summary
- dependency expansion
- material-gap stop gate

### F — Skill Invocation Contract

**Status:** Done

- activation, precedence, lifecycle, statuses, and fallback
- phase-by-phase skill loading
- invocation evidence

### G — Quality Gate Skill

**Status:** Done

- profile selection
- exact command/evidence collection
- compliance and traceability review
- binary documentation-readiness decision

### H — README Consolidation

**Status:** Done

- complete architecture map
- docs/skills/templates/tooling index
- consumer installation layout

### I — Skill Consistency Audit

**Status:** Done

- canonical `SKILL_TEMPLATE`
- normalized skill section order
- one primary phase owner per skill
- explicit cross-phase handoffs
- reduced normative duplication

### J — Automated Skills and Docs Audit

**Status:** Done

Implemented:

- `agentic_audit`
- toolkit and consumer scope auto-detection
- required-file checks
- canonical skill-schema checks
- unique skill-title checks
- registry/file synchronization
- phase ownership-map checks
- README index checks
- repository-local reference checks
- source/consumer template mapping checks
- AGENTS-to-skill link checks
- `docs/AGENTIC_AUDIT.md`
- conditional audit gate in `AGENTS.md`
- quality-skill and quality-doc integration
- executable script mode
- `.github/workflows/agentic-audit.yml`

Current CI evidence:

```text
Workflow: Agentic workflow audit
Run: 30012816440
Commit: 33390cb918c9af95aea0529561ad63425fc98736
Conclusion: success
Steps:
- Check Ruby syntax: success
- Run toolkit audit: success
```

The audit is conditional for agentic surfaces. Ordinary feature changes do not receive an unnecessary additional gate.

## Preserved Invariants

The migration did not intentionally alter:

- API envelope and status-code rules
- Blueprinter ownership
- Pundit/Ransack/Kaminari conventions
- requirement read-only policy
- requirement-owned versions
- Flow/PRD ownership and structure
- verification profile semantics
- API contract-audit behavior
- final evidence requirements

## Remaining K — Final Integration Audit

**Status:** Next

Final work:

- compare the complete PR against `main`
- inspect all changed files and commit/check status
- verify AGENTS, docs, skills, templates, scripts, and CI agree
- run/inspect latest automated audit evidence on final head
- correct residual stale wording or broken ownership
- update PR title/body to final umbrella scope
- synchronize this migration doc and `changelog.md`
- document checks that were executed and checks unavailable in this toolkit-only repository
- leave PR ready for owner review/merge without merging automatically
