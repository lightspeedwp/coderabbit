# Implementation Plan: Central Review Configuration Completion

**Branch**: `001-central-review-config` (implemented on git branch `feature/git-2492-automation-coderabbit-complete-the-central-review`) | **Date**: 2026-09-30 | **Spec**: [spec.md](./spec.md)

**Input**: Feature specification from `/specs/001-central-review-config/spec.md`

**Note**: This template is filled in by the `/speckit-plan` command; its definition describes the execution workflow.

## Summary

Add three settings to the central `lightspeedwp/coderabbit/.coderabbit.yaml` — a `reviews.tools` block disabling `phpcs`/`phpstan`/`phpmd` while enabling `gitleaks`/`trufflehog`, `reviews.auto_review.base_branches` covering `main`/`develop`/`feature/.*`/`fix/.*`, and `knowledge_base.automatic_linking_mode: auto`. This is a single-file configuration edit with no application code and no PR/commit/verification steps of its own — the repository owner handles committing, opening PRs, and verifying the resolved configuration entirely outside this feature, by hand (see spec Assumptions).

## Technical Context

**Language/Version**: N/A — this feature is a YAML configuration edit, not application code.

**Primary Dependencies**: None beyond editing the existing file directly.

**Storage**: N/A.

**Testing**: Out of scope for this feature entirely — no testing or verification is performed as part of implementation (see spec Assumptions).

**Target Platform**: N/A — this feature only edits a file on disk; no deployment, PR, or platform interaction happens as part of it.

**Project Type**: Configuration change (single file) — not a library/CLI/service, and not paired with any procedure beyond the edit itself.

**Performance Goals**: N/A.

**Constraints**: Exactly one file changed (`.coderabbit.yaml`); no commits, pushes, PRs, or verification of any kind as part of this feature's implementation.

**Scale/Scope**: 1 file changed at the central repo. 0 PRs opened, 0 commits pushed, 0 downstream repos touched — all explicitly out of scope (see spec Assumptions).

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

`.specify/memory/constitution.md` is still the blank Spec Kit template (intentionally — see the Spec Kit installation PR for this repo) with no ratified principles yet. There are no gates to evaluate against. No violations possible; nothing to justify in Complexity Tracking.

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit-plan command output)
├── research.md          # Phase 0 output (/speckit-plan command)
├── data-model.md        # Phase 1 output (/speckit-plan command)
└── tasks.md             # Phase 2 output (/speckit-tasks command - NOT created by /speckit-plan)
```

No `contracts/` or `quickstart.md` — both described PR/verification procedures that are explicitly out of scope for this feature (see spec Assumptions). The target config fields are documented directly in `data-model.md` instead.

### Source Code (repository root)

```text
lightspeedwp/coderabbit/
└── .coderabbit.yaml      # The single file this feature modifies
                           # (reviews.tools, reviews.auto_review.base_branches,
                           # knowledge_base.automatic_linking_mode)
```

**Structure Decision**: This repo is a CodeRabbit configuration repository, not an application — there is no `src/`/`tests/` split. The entire feature is one edit to the existing root-level `.coderabbit.yaml`. Nothing else — no commit, PR, or verification step belongs to this feature's implementation (see spec Assumptions).

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

Not applicable — no Constitution Check violations (no ratified constitution gates exist yet in this repo).
