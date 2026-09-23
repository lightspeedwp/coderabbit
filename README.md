# coderabbit

## Local setup

```bash
mkdir coderabbit
cd coderabbit
git init -b main
mkdir -p guidelines
cat > guidelines/block-plugin.md <<'DOC'
# WordPress block plugin guidelines

Use block.json as the canonical metadata source. Prefer Block API v3, use `useBlockProps()` on the edit root, keep strings translatable with `__()`, and sanitise and escape dynamic output in PHP render callbacks and REST endpoints.
DOC
cat > guidelines/block-theme.md <<'DOC'
# WordPress block theme guidelines

Use `theme.json` for global styles, keep templates composable and block-first, prefer native blocks and style variations, and enforce sanitisation, escaping, accessibility, and minimal front-end JavaScript.
DOC
git add guidelines
git commit -m "docs: add WordPress CodeRabbit guidelines"
git remote add origin https://github.com/lightspeedwp/coderabbit.git
git fetch origin
git branch -M main
```

The production fallback CodeRabbit configuration lives in [`.coderabbit.yaml`](./.coderabbit.yaml).
