# CodeRabbit

[CodeRabbit](https://www.coderabbit.ai) is an AI-powered code review tool that helps development teams improve code quality and accelerate the review process. It integrates with popular version control platforms and provides intelligent feedback on code changes.

This curated list covers the best resources, tutorials, and community content related to CodeRabbit 🐰

## Contents

- [Local Setup](#local-setup)
- [Official Resources](#official-resources)
- [Getting Started](#getting-started)
- [API Reference](#api-reference)
- [Configuration Examples](#configuration-examples)
- [Pre-Merge Check Packs](#pre-merge-check-packs)
- [Integration Guides](#integration-guides)
- [Video Tutorials](#video-tutorials)
- [Blogs](#blogs)

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

## Official Resources

- [Documentation](https://docs.coderabbit.ai) - Comprehensive docs covering all aspects of CodeRabbit.
- [Blog](https://www.coderabbit.ai/blog) - Official blog featuring updates, tutorials, and best practices.
- [FAQ](https://www.coderabbit.ai/faq) - Frequently asked questions about CodeRabbit.
- [GitHub Repository](https://github.com/coderabbitai/ai-pr-reviewer) - Official AI PR Reviewer repository.
- [LinkedIn](https://www.linkedin.com/company/coderabbitai/) - Official LinkedIn presence.
- [Twitter](https://x.com/coderabbitai) - Official Twitter/X account.
- [YouTube Channel](https://www.youtube.com/@CodeRabbitAI) - Official YouTube channel with tutorials and updates.

## Getting Started

- [CodeRabbit Startup Program](https://www.coderabbit.ai/blog/coderabbit-startup-program) - Special program for startups.
- [AI Code Reviewer Examples](https://www.coderabbit.ai/blog/how-to-use-an-ai-code-reviewer-on-github-in-4-examples) - Four practical examples of using CodeRabbit.

## API Reference

- [OpenAPI Documentation](https://docs.coderabbit.ai/api-reference/) - Complete Swagger documentation for CodeRabbit's REST API endpoints.

## Configuration Examples

### Enterprise Configuration Example

Explore real-world CodeRabbit configurations from various projects.

```yaml
# yaml-language-server: $schema=https://coderabbit.ai/integrations/schema.v2.json
language: "en-US"
early_access: false
tone_instructions: 'You are an expert code reviewer in Java, TypeScript, JavaScript, and NodeJS. You work in an enterprise software developer team, providing concise and clear code review advice. You only elaborate or provide detailed explanations when requested.'
reviews:
  profile: "chill"
  request_changes_workflow: false
  high_level_summary: true
  poem: true
  review_status: true
  collapse_walkthrough: false
  auto_review:
    enabled: true
    drafts: false
    base_branches: ["pg", "release"]
  path_instructions:
    - path: "app/client/cypress/**/**.*"
      instructions: |
        Review the following e2e test code written using the Cypress test library. Ensure that:
        - Follow best practices for Cypress code and e2e automation
        - Avoid using cy.wait in code
        - Avoid using cy.pause in code
        - Avoid using agHelper.sleep()
        - Use locator variables for locators
        - Use data-* attributes for selectors
        - Avoid Xpaths, Attributes and CSS path
        - Avoid selectors like .btn.submit
        - Perform logins via API
        - Avoid using it.only
        - Use multiple assertions
        - Avoid string assertions
        - Ensure unique filenames
chat:
  auto_reply: true
```

Find more examples in the [`configs/`](configs/) directory, organized by language:

```
configs/
├── javascript/   # JavaScript project configurations
├── typescript/   # TypeScript project configurations
├── python/       # Python project configurations
├── go/          # Go project configurations
└── multi-language/ # Full-stack project configurations
```

## Pre-Merge Check Packs

The [pre-merge check catalog](configs/pre-mergechecks/README.md) contains
copyable policy examples for common engineering risks, multi-tenant SaaS,
fintech, and health tech. It explains how to select, adapt, and safely introduce
individual checks without replacing existing repository configuration.

## Integration Guides

- [CI/CD Pipeline Integration](https://www.coderabbit.ai/blog/how-to-run-static-analysis-on-your-ci-cd-pipelines-using-ai) - Adding AI-powered static analysis to CI/CD pipelines.
- [Linear Board Integration](https://www.coderabbit.ai/blog/how-to-use-coderabbit-to-validate-issues-against-linear-board) - Guide for Linear board integration.
- [DevOps Pipeline Integration](https://www.coderabbit.ai/blog/how-to-integrate-ai-code-review-into-your-devops-pipeline) - Comprehensive DevOps integration guide.

## Video Tutorials

- [Getting Started Tutorial](https://www.youtube.com/watch?v=3SyUOSebG7E) - Official step-by-step guide for new users.

## Blogs

- [AI Can Make a Code Review for Free](https://tomaszs2.medium.com/ai-can-make-a-code-review-for-free-a559cf74efa5)
- [CodeRabbit Deep Dive](https://www.coderabbit.ai/blog/coderabbit-deep-dive)
- [CodeRabbit vs Others: AI Code Review Tools](https://www.devtoolsacademy.com/blog/coderabbit-vs-others-ai-code-review-tools)
- [Why Developers Hate Linters](https://www.coderabbit.ai/blog/why-developers-hate-linters)
- [How to Automate TypeScript Code Reviews with CodeRabbit](https://www.coderabbit.ai/blog/how-to-automate-typescript-code-reviews-with-coderabbit)
