---

description: "Task list for: Central Review Configuration Completion"
---

# Tasks: Central Review Configuration Completion

**Input**: Design documents from `/specs/001-central-review-config/`

**Prerequisites**: plan.md, spec.md, research.md, data-model.md

**Tests**: No test tasks — testing/verification is explicitly out of scope for this feature (see spec Assumptions). The repository owner handles verification manually, entirely outside this task list, tracked separately as GIT-2493.

**Scope note**: This feature is the `.coderabbit.yaml` edit only. It does **not** commit, push, open a PR, or verify anything — those are explicitly out of scope (see spec Assumptions) and are not represented anywhere in this task list.

**Organization**: Tasks are grouped by user story per spec.md. Note: User Stories 1 and 2 both edit the *same* single file (`.coderabbit.yaml`), so tasks here are **not** parallelizable across stories — see the Parallel Opportunities note below.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (US1, US2)
- Every task includes the exact file path

## Path Conventions

Single file: `.coderabbit.yaml` at the root of `lightspeedwp/coderabbit`. No `src/`/`tests/` split — see plan.md Structure Decision.

---

## Phase 1: Setup

**Purpose**: Confirm the branch is in a known-good state before editing the shared config file.

- [ ] T001 Confirm the current branch (`feature/git-2492-automation-coderabbit-complete-the-central-review`) is up to date with `origin/develop` and the working tree is clean, in `lightspeedwp/coderabbit`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Both user stories edit the same file — confirm its current structure before any edits, so insertions land in the right place without clobbering existing content.

**⚠️ CRITICAL**: No user story task can begin until this phase is complete.

- [ ] T002 Read the current `.coderabbit.yaml` in `lightspeedwp/coderabbit` and confirm the exact insertion points for a new `reviews.tools` block, the existing `reviews.auto_review` block, and a new `knowledge_base.automatic_linking_mode` field, per [data-model.md](./data-model.md)

**Checkpoint**: File structure confirmed — user story edits can now proceed.

---

## Phase 3: User Story 1 - Eliminate duplicate PHP linting while keeping secret scanning active (Priority: P1) 🎯 MVP

**Goal**: `phpcs`, `phpstan`, `phpmd` disabled; `gitleaks`, `trufflehog` enabled — per FR-001, FR-002.

**Independent Test**: Read the file after this task and confirm the five tool states listed in [data-model.md](./data-model.md).

### Implementation for User Story 1

- [ ] T003 [US1] Add a `reviews.tools` block to `lightspeedwp/coderabbit/.coderabbit.yaml` with `phpcs.enabled: false`, `phpstan.enabled: false`, `phpmd.enabled: false`, `gitleaks.enabled: true`, `trufflehog.enabled: true`, exactly as specified in [data-model.md](./data-model.md)

**Checkpoint**: `reviews.tools` block is in place.

---

## Phase 4: User Story 2 - Extend automatic review coverage to stacked branches and related repositories (Priority: P2)

**Goal**: `base_branches` covers stacked PRs; `automatic_linking_mode` enables cross-repo awareness — per FR-003, FR-004.

**Independent Test**: Read the file after this task and confirm `base_branches` and `automatic_linking_mode` as listed in [data-model.md](./data-model.md).

### Implementation for User Story 2

- [ ] T004 [US2] Add `main`, `develop`, `feature/.*`, `fix/.*` to `reviews.auto_review.base_branches` in `lightspeedwp/coderabbit/.coderabbit.yaml`, using regex syntax (not glob) per [research.md](./research.md)
- [ ] T005 [US2] Add `knowledge_base.automatic_linking_mode: auto` to `lightspeedwp/coderabbit/.coderabbit.yaml`, per [data-model.md](./data-model.md)

**Checkpoint**: All four config additions (T003–T005) are now in place. Feature complete — the file is ready for the repository owner to commit, open a PR, and verify manually, entirely outside this task list.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies.
- **Foundational (Phase 2)**: Depends on Setup — blocks both user stories.
- **User Story 1 (Phase 3)**: Depends on Foundational.
- **User Story 2 (Phase 4)**: Depends on Foundational. Does not depend on US1, but shares the same file — do not run concurrently with US1's edit.

### Parallel Opportunities

None — T003, T004, and T005 all edit the same file (`lightspeedwp/coderabbit/.coderabbit.yaml`) and must be applied sequentially to avoid conflicting edits. No `[P]` markers apply anywhere in this task list.

---

## Implementation Strategy

Apply T001–T005 in order. That's the entire feature. Do not commit, push, open a PR, or attempt any verification — all of that is the repository owner's own manual follow-up, outside this task list's scope (see spec Assumptions).

Note: GIT-2493 (verification) and GIT-2494 (adding `inheritance: true` to `ls-theme` and `ls-plugin`) are both explicitly out of scope for this feature — see spec.md Assumptions — and are not represented in this task list.
