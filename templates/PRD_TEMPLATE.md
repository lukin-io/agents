# PRD Template

Stable structure exemplar for feature-owned PRD docs under `doc/prd/**`.

This template must mirror the current canonical feature-owned PRD format represented by `doc/prd/PREFERENCES.md`.
Use placeholders, but keep the same section order and document shape.

````text
# frozen_string_literal: true
---
title: <Feature> PRD
description: Product requirements for <requirement version or feature scope summary>.
status: Implemented
updated: YYYY-MM-DD
type: PRD (Product Requirements Document) -> high-level product + integration reference that clarifies scope, contracts, acceptance criteria, and rollout expectations across engineering and product.
---

**Requirement source:** `doc/requirements/...`
**Canonical PRD doc:** `doc/prd/...`
**Related Flow doc:** `doc/flow/...`
**Latest implemented requirement version:** `Version ...`
**Last updated by task:** `TASK_ID`
**Related changelog fragments:** `changelogs/unreleased/TASK_ID.md` (optional)

## Recent Implemented History
- `Version ...` implemented by `TASK_ID`: short PRD-style summary.
- `Version ...` implemented by `TASK_ID`: short PRD-style summary.
- `Version ...` implemented by `legacy task ID not recorded`: short PRD-style summary.

## 1. Problem Statement
Short product problem statement.

## 2. Goals & Non-Goals
### Goals
- Goal 1
- Goal 2

### Non-Goals
- Non-goal 1
- Non-goal 2

## 3. Scope
- **In scope:**
  - endpoint / surface / system
- **Out of scope:**
  - excluded surface / task

## 4. User Stories
1. As a user, ...
2. As an admin, ...
3. As an integrator, ...

## 5. Flows & UX Notes
### Primary flow
1. Step one
2. Step two
3. Step three

### UX notes
- Constraint or note
- Edge-case behavior

## 6. Technical Considerations
- Controller / service ownership
- Blueprint / contract ownership
- Configuration / fallback notes
- Persistence or compatibility note

## 7. Security & Compliance
- Authentication rule
- Authorization rule
- Contract / envelope / validation safety note

## 8. Acceptance Criteria
- Acceptance criterion 1
- Acceptance criterion 2
- Acceptance criterion 3

## 9. Metrics & KPIs
- KPI 1
- KPI 2

## 10. Rollout / Launch Plan
- **Phase 1:** implementation
- **Phase 2:** testing / contract verification
- **Phase 3:** docs / rollout

## 11. Open Questions
- Question 1
- Question 2

## 12. Dependencies / Prerequisites
- Requirement contract
- Existing models / services
- Shared infra / config

## 13. Version History
- `Version ...` implemented by `TASK_ID` (YYYY-MM-DD): short PRD-style summary written in product/scope language.
- `Version ...` implemented by `TASK_ID` (YYYY-MM-DD): short PRD-style summary written in product/scope language.
- `YYYY-MM-DD` implemented by `TASK_ID`: dated non-numeric summary when the requirement doc is unversioned.

## Drift Delta

Added:
- New PRD scope, stories, or acceptance coverage.

Changed:
- Updated rollout, scope, or integration framing.

Removed:
- None.
````
