# Feature Specification: Central Review Configuration Completion

**Feature Branch**: `feature/git-2492-automation-coderabbit-complete-the-central-review`

**Created**: 2026-09-30

**Status**: Draft

**Input**: User description: "Complete the central CodeRabbit review configuration in lightspeedwp/coderabbit/.coderabbit.yaml: add a tools: block disabling phpcs, phpstan, and phpmd (duplicate of existing local PHP linting) while enabling gitleaks and trufflehog secret scanning; add auto_review.base_branches covering main, develop, feature/.*, and fix/.* so stacked PRs get reviewed; and set knowledge_base.automatic_linking_mode: auto to enable cross-repository awareness."

## Clarifications

### Session 2026-09-30

- Q: Should "verified" mean confirming the four settings via `@coderabbitai configuration` on a test PR against the central repo itself, or also demonstrating each setting's real-world behavioral effect? → A: Config-level verification only for this feature (Option C). *(Superseded — see the second clarification below: verification of any kind, including config-level, is out of scope for this feature entirely.)*
- Q (follow-up): Should this feature's implementation include opening PRs or running verification at all? → A: No. This feature covers only the `.coderabbit.yaml` edits themselves. Opening PRs and verifying the resolved configuration is done manually by the repository owner, entirely outside this spec's implementation scope — not a task this feature's implementation performs.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Eliminate duplicate PHP linting while keeping secret scanning active (Priority: P1)

As a maintainer of a repository that inherits the central CodeRabbit configuration, I don't want CodeRabbit's PHP linting tools raising findings that duplicate what local/build-time linting already catches, but I do want secret scanning to remain active so credential leaks are still caught automatically.

**Why this priority**: This is the core problem motivating the change — without it, every inheriting repo gets redundant PHP findings on every PR, which was explicitly identified as unwanted noise.

**Independent Test**: Read `lightspeedwp/coderabbit/.coderabbit.yaml` after the change and confirm `reviews.tools.phpcs.enabled`, `reviews.tools.phpstan.enabled`, and `reviews.tools.phpmd.enabled` are set to `false`, and `reviews.tools.gitleaks.enabled`/`reviews.tools.trufflehog.enabled` are set to `true`. (Confirming this against CodeRabbit's actual resolved behavior, and demonstrating it in an inheriting repository, is a separate manual step the repository owner performs afterward — not part of this feature.)

**Acceptance Scenarios**:

1. **Given** the central configuration file is edited, **When** the file is read, **Then** `phpcs`, `phpstan`, and `phpmd` all show `enabled: false`.
2. **Given** the central configuration file is edited, **When** the file is read, **Then** `gitleaks` and `trufflehog` both show `enabled: true`.

---

### User Story 2 - Extend automatic review coverage to stacked branches and related repositories (Priority: P2)

As a maintainer working across paired repositories for the same site (e.g. a plugin and its companion theme), I want automatic review coverage to include stacked PRs on `feature/*`/`fix/*` branches, not just PRs targeting `main`/`develop`, and I want CodeRabbit able to recognize when a change in one repository likely affects a related one.

**Why this priority**: Without this, stacked PRs are silently skipped by automatic review, and cross-repository impact goes unnoticed — both were identified as real, current gaps.

**Independent Test**: Read `lightspeedwp/coderabbit/.coderabbit.yaml` after the change and confirm `reviews.auto_review.base_branches` includes `main`, `develop`, `feature/.*`, and `fix/.*`, and `knowledge_base.automatic_linking_mode` is set to `auto`. (Confirming this against CodeRabbit's actual resolved behavior is a separate manual step the repository owner performs afterward — not part of this feature.)

**Acceptance Scenarios**:

1. **Given** the central configuration file is edited, **When** the file is read, **Then** `base_branches` includes `main`, `develop`, `feature/.*`, and `fix/.*`.
2. **Given** the central configuration file is edited, **When** the file is read, **Then** `automatic_linking_mode` shows `auto`.

### Edge Cases

- What happens for a repository that inherits the central configuration but hasn't opted into `inheritance: true`? Central defaults don't apply until a repository explicitly opts in — this is tracked separately (GIT-2494) and is out of scope here.
- What happens for a repository whose branches don't follow the `feature/*`/`fix/*` naming convention? Those branches aren't covered by this `base_branches` list and would need their own explicit configuration.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The central configuration MUST set `reviews.tools.phpcs.enabled`, `reviews.tools.phpstan.enabled`, and `reviews.tools.phpmd.enabled` to `false`.
- **FR-002**: The central configuration MUST set `reviews.tools.gitleaks.enabled` and `reviews.tools.trufflehog.enabled` to `true`.
- **FR-003**: The central configuration MUST set `reviews.auto_review.base_branches` to include `main`, `develop`, `feature/.*`, and `fix/.*`.
- **FR-004**: The central configuration MUST set `knowledge_base.automatic_linking_mode` to `auto`.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: `lightspeedwp/coderabbit/.coderabbit.yaml`, when read, shows `phpcs`, `phpstan`, and `phpmd` all as `enabled: false`.
- **SC-002**: The file shows `gitleaks` and `trufflehog` both as `enabled: true`.
- **SC-003**: The file shows `base_branches` including `main`, `develop`, `feature/.*`, and `fix/.*`.
- **SC-004**: The file shows `automatic_linking_mode` as `auto`.

## Assumptions

- The CodeRabbit GitHub App is already installed and functioning on `lightspeedwp/coderabbit` (confirmed in prior work on this project) — not re-verified by this feature.
- Repositories wanting these central defaults must separately opt in via `inheritance: true` — that rollout is tracked as GIT-2494 and is explicitly out of scope for this feature.
- The review profile (`assertive`) is unchanged by this feature — only the `tools`, `base_branches`, and `automatic_linking_mode` settings are affected.
- **Out of scope for this feature entirely**: opening any PR, committing, pushing, or verifying the resolved configuration against CodeRabbit's live behavior (via `@coderabbitai configuration` or any other means). This feature is the YAML edit only. All PR creation and verification — including throwaway/test PRs — is done manually by the repository owner (tracked separately as GIT-2493), never as part of this feature's implementation.
