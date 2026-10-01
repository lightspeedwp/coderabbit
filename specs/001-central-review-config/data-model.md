# Phase 1 Data Model: Central Review Configuration Completion

No data entities apply to this feature. It modifies three existing fields in one YAML configuration file (`lightspeedwp/coderabbit/.coderabbit.yaml`) — there is no persisted data, no user-facing data structure, and no state beyond the configuration values themselves.

## Target configuration fields

The exact fields and values this feature adds, per FR-001–FR-004:

```yaml
reviews:
  tools:
    phpcs:
      enabled: false
    phpstan:
      enabled: false
    phpmd:
      enabled: false
    gitleaks:
      enabled: true
    trufflehog:
      enabled: true

  auto_review:
    base_branches:
      - "main"
      - "develop"
      - "feature/.*"
      - "fix/.*"

knowledge_base:
  automatic_linking_mode: auto
```

- `phpcs`, `phpstan`, `phpmd` — disabled. Duplicate existing local/build-time PHP linting.
- `gitleaks`, `trufflehog` — enabled. Secret scanning stays active.
- `base_branches` — regex syntax, not glob (`feature/.*`/`fix/.*`, not `feature/*`/`fix/*` — see [research.md](./research.md) for why).
- `automatic_linking_mode: auto` — privacy-aware progressive cross-repo discovery.

These fields are inserted into the existing `reviews:` and `knowledge_base:` blocks already present in the file — not new top-level sections.
