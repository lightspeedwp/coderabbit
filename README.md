# coderabbit

## Local setup

```bash
mkdir coderabbit
cd coderabbit
git init -b main
mkdir -p guidelines
cat > guidelines/block-plugin.md <<'DOC'
# WordPress block plugin guidelines
DOC
cat > guidelines/block-theme.md <<'DOC'
# WordPress block theme guidelines
DOC
git add guidelines
git commit -m "docs: add WordPress CodeRabbit guidelines"
git remote add origin https://github.com/lightspeedwp/coderabbit.git
git fetch origin
git branch -M main
```

The production fallback CodeRabbit configuration lives in [`/home/runner/work/coderabbit/coderabbit/.coderabbit.yaml`](./.coderabbit.yaml).
