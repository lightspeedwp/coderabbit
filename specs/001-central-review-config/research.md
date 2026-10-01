# Phase 0 Research: Central Review Configuration Completion

No `[NEEDS CLARIFICATION]` markers remained in the Technical Context after `/speckit-specify` and `/speckit-clarify` — this feature's scope and mechanism were already established through prior work on this project (the GIT-2407 audit, the CodeRabbit org-config meeting, and live testing on `ls-theme` PR #63). This file records the evidence behind each field/value decision rather than open research questions. It does not cover verification method, since verification is explicitly out of scope for this feature (see spec Assumptions) — the repository owner handles that manually, tracked separately as GIT-2493.

## Decision: Tool activation via `reviews.tools.<name>.enabled`

**Decision**: Use `reviews.tools.phpcs.enabled: false`, `reviews.tools.phpstan.enabled: false`, `reviews.tools.phpmd.enabled: false`, `reviews.tools.gitleaks.enabled: true`, `reviews.tools.trufflehog.enabled: true`.

**Rationale**: Confirmed live via `@coderabbitai configuration` on `ls-theme` PR #63 that `gitleaks` and `trufflehog` are recognized, structured tool keys in the resolved configuration (same format as every other officially documented tool) — not a silent no-op, despite `gitleaks` being absent from CodeRabbit's published Tool Catalog docs page. `phpcs`/`phpstan`/`phpmd` are enabled by default (`Source: defaults`) unless explicitly disabled.

**Alternatives considered**: Relying on the *absence* of a `phpcs.xml` file to implicitly suppress `phpcs` — rejected. It only affects `phpcs` specifically; `phpstan` auto-generates a temporary config and runs regardless, and `phpmd` is enabled by default too. Explicit `enabled: false` is deterministic and doesn't depend on file presence/absence that could change for unrelated reasons later.

## Decision: `base_branches` uses regex syntax, not glob

**Decision**: `reviews.auto_review.base_branches: ["main", "develop", "feature/.*", "fix/.*"]`.

**Rationale**: CodeRabbit's own autofix corrected this exact mistake on `ls-theme`'s config previously (commit `2bffd45`) — `feature/*` and `fix/*` are invalid regex (nothing to repeat before `*`), so they silently matched nothing. `feature/.*` and `fix/.*` are the confirmed-working regex equivalents.

**Alternatives considered**: Glob-style `feature/*`/`fix/*` — rejected, proven non-functional by CodeRabbit's own prior autofix on this exact pattern.

## Decision: `automatic_linking_mode: auto`

**Decision**: `knowledge_base.automatic_linking_mode: auto`.

**Rationale**: `auto` is privacy-aware progressive discovery (for a public repo, only considers other public repos) and matches what's already documented as the recommended default in this repo's own rollout docs (`Phased Rollout Plan`, `Centralised Org Config` tabs). Directly serves the stated need of surfacing relationships between paired repos (e.g. `ls-plugin` → `ls-theme`).

**Alternatives considered**: `enabled` — rejected, skips the privacy-aware distinction with no stated need for it. `disabled` — rejected, it's the current default and produces no change (the actual problem being solved).

## Out of scope: verification method

Verifying the resolved configuration (e.g. via `@coderabbitai configuration` on a test PR, the method already used successfully on `ls-theme` PR #63 and `coderabbit` PR #119) is explicitly **not** part of this feature. The repository owner performs verification manually, entirely outside this feature's implementation — see spec Assumptions and GIT-2493.
