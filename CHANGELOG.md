# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased] — Complete central review configuration: tools, base_branches, linking (GIT-2492)

### Added

- `.coderabbit.yaml`: added a `reviews.tools` block disabling `phpcs`, `phpstan`, and `phpmd` (duplicate existing local PHP linting) while enabling `gitleaks` and `trufflehog` secret scanning.
- `.coderabbit.yaml`: added `reviews.auto_review.base_branches` covering `main`, `develop`, `feature/.*`, and `fix/.*` (regex syntax) so stacked pull requests on feature/fix branches are reviewed automatically, not just PRs into `main`/`develop`.
- `.coderabbit.yaml`: added `knowledge_base.automatic_linking_mode: auto`, enabling CodeRabbit to recognise when a change in one repository likely affects a related one (e.g. `ls-plugin` → `ls-theme`).
- `specs/001-central-review-config/`: Spec Kit specification, clarifications, implementation plan, research notes, data model, requirements checklist, and tasks documenting this change, produced via `/speckit-specify` → `/speckit-clarify` → `/speckit-plan` → `/speckit-tasks` → `/speckit-implement`.

---

## [Unreleased] — Install Spec Kit (speckit) for Copilot and Claude (#119)

### Added

- `.specify/`: Spec Kit scripts, templates, workflow registry, and integration manifests, mirroring the install already in use in `ls-theme`.
- `.github/skills/speckit-*/SKILL.md`: the Copilot integration (10 skills — analyze, checklist, clarify, constitution, converge, implement, plan, specify, tasks, taskstoissues).
- `.claude/skills/speckit-*/SKILL.md`: the same 10 skills for the Claude Code integration.
- `.specify/memory/constitution.md`: the blank Spec Kit constitution template, left for the team to fill in with this repository's own principles (ls-theme's constitution is theme-specific and doesn't apply here).

---

## [Unreleased] — Add starter configs to the repo and some resources

### Added

- `configs/`: a library of example CodeRabbit configurations — `starter/` (barebones, initial, quiet, sample), `wordpress/`, `monorepo/`, `fullstack/`, `docs/`, `testing/`, and `github/reviewpad.yaml`.
- `configs/pre-mergechecks/`: ten ready-to-adapt policy packs (security, fintech, health tech, multi-tenant SaaS, infrastructure, data privacy, performance, PR hygiene, testing, quality) plus a README explaining how to adopt them safely.
- `schema.v2.json`: the CodeRabbit configuration JSON schema, vendored locally for reference.
- `README.md`: expanded with local setup instructions and links to the configuration examples.

---

## [Unreleased] — Scaffold central CodeRabbit fallback config for WordPress block repos (#1)

### Added

- `.coderabbit.yaml`: the initial central fallback configuration (schema v2), covering language/tone, review workflow settings, auto-review with incremental reviews and a 5-commit pause threshold, WIP/skip-review and common-bot exclusions, WordPress-specific path instructions (Gutenberg Block API v3, `block.json`, PHP/WPCS), knowledge-base guideline sourcing, and finishing touches (autofix, fix CI, merge conflict resolution, unit test generation, simplify, docstrings).
- `guidelines/block-plugin.md`: standardised review guidelines for WordPress block plugins (PHP, REST API, React/Gutenberg).
- `guidelines/block-theme.md`: standardised review guidelines for WordPress block themes (`theme.json`, HTML templates, CSS/SCSS).
- `README.md`: initial bootstrap documentation for setting up the central repository.
