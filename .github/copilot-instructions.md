# Copilot Instructions

## Repository Overview

This repository (`org-test-reply-1/test-runner`) contains a .NET sample application and the GitHub Actions workflows used to build, test, and monitor it.

## Workflow Architecture

- **`ai.yml`** (`test-runner-gabri`): Scheduled every 2 hours; calls the centralised `platform-ai-workflows/ai-failure-handler.yml` template to scan the org for failed runs and open issues.
- **`ai-enhanced-pipeline.yml`**: Triggers on push/PR; runs an AI-powered security scan via `platform-ai-workflows/ai-security-scan.yml`. The scan is non-blocking (`continue-on-error: true`).
- **`ai-summary.yml`**: Daily summary of changes, sent via Teams.
- **`pipeline.yml`**: App build & test pipeline using the centralised `platform-workflows/ci-dotnet.yml` template.

## Conventions

- **Do not modify central templates** in `platform-ai-workflows` or `platform-workflows` from this repo. Report issues upstream instead.
- Workflow failures detected by `test-runner-gabri` automatically create issues labelled `copilot-auto` + `bug`, with Copilot assigned.
- Each issue describes the failing workflow run, job/step details, and log excerpts to help identify the root cause.

## Common Failure Patterns

| Symptom | Likely Cause | Action |
|---|---|---|
| Bash syntax error in `ai-failure-handler` | Bug in central template | Report to `platform-ai-workflows` maintainers |
| `Field 'id' doesn't exist on type 'Actor'` (GraphQL) | GitHub API schema change | Report to `platform-ai-workflows` maintainers |
| Security scan fails | Missing `COPILOT_GITHUB_TOKEN` secret or agent file | Check secrets; scan is non-blocking so pipeline still passes |
| `.github/agents/*.agent.md` not found | Agent file missing in repo | Add the required agent file |

## Secrets Required

| Secret | Used By |
|---|---|
| `COPILOT_ASSIGN_PAT` | `ai.yml` → issue creation and Copilot assignment |
| `TEAMS_WEBHOOK_URL` | `ai.yml` → Teams notifications |
| `COPILOT_GITHUB_TOKEN` | `ai-enhanced-pipeline.yml` → AI security scan (optional) |
