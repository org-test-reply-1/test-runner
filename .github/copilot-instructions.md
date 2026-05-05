# Copilot Instructions

## Repository Overview

This repository is a test runner for the `org-test-reply-1` organization. It contains:
- `.github/workflows/` – GitHub Actions workflows (including the `test-runner-gabri` failure scanner)
- `samples/` – Sample application code
- `tests/` – Test suites

## Workflow Architecture

| Workflow | Purpose |
|---|---|
| `ai.yml` | Scheduled org-wide failure scanner (calls `platform-ai-workflows/ai-failure-handler`) |
| `ai-enhanced-pipeline.yml` | AI-powered CI/CD pipeline (calls `platform-ai-workflows/ai-security-scan`) |
| `ai-summary.yml` | Daily digest of changes (calls `platform-ai-workflows/ai-daily-summary`) |
| `pipeline.yml` | Application build & test pipeline (calls `platform-workflows/ci-dotnet`) |
| `pipeline-test.yaml` | Pipeline integration tests |

All platform templates live in `org-test-reply-1/platform-ai-workflows` and `org-test-reply-1/platform-workflows`. This repo consumes them via `uses: …@main`.

## Conventions

- **Do not modify** centralized workflow templates in `platform-ai-workflows` or `platform-workflows` from within this repo. Open a separate issue/PR in the platform repo if a template fix is needed.
- If an auto-created issue references a failing workflow step inside a platform template, confirm the fix is already applied to the template (it uses `@main`, so next run picks it up automatically).
- Secrets `COPILOT_ASSIGN_PAT` and `TEAMS_WEBHOOK_URL` must be configured in repository settings for `ai.yml` and `ai-summary.yml` to work.
- The sample application under `samples/dotnetapp` is built with .NET. Run tests with `dotnet test` from that directory.

## Common Issues

### `gh: Field 'id' doesn't exist on type 'Actor'`

GitHub's GraphQL `Actor` interface does not expose an `id` field directly. Use inline type fragments instead:

```graphql
nodes {
  login
  ... on Bot { id }
  ... on User { id }
  ... on Mannequin { id }
}
```

This fix was applied to `platform-ai-workflows/ai-failure-handler.yml` — no changes needed in this repo.
