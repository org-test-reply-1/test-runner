# Test Runner

GitHub Enterprise Organization Structure

## Repository Organization

```
GitHub Enterprise Organization
│
├── platform-workflows/          ← Team Platform (template CI/CD riutilizzabili)
│   └── .github/
│       └── workflows/
│           ├── ci-dotnet.yml           ← Reusable Workflow per build .NET
│           ├── ci-react-vite.yml       ← Reusable Workflow per build React Vite
│           ├── docker-build-push.yml
│           ├── deploy-arc.yml
│           └── tag.yml
│
├── platform-actions/            ← Team Platform (Composite Actions riutilizzabili)
│   ├── copilot-cli-setup/
│   │   └── action.yml
│   ├── copilot-run-agent/
│   │   └── action.yml
│   ├── create-namespaces/
│   │   └── action.yml
│   ├── deploy-arc-controller/
│   │   └── action.yml
│   ├── deploy-arc/
│   │   └── action.yml
│   ├── docker-build-push/
│   │   └── action.yml
│   ├── notify-teams/
│   │   └── action.yml
│   ├── react-vite-build/
│   │   └── action.yml
│   └── tag-repo/
│       └── action.yml
│
├── platform-ai-workflows/       ← Team Platform (workflow AI riutilizzabili)
│   └── .github/
│       └── workflows/
│           ├── ai-daily-summary.yml
│           ├── ai-failure-handler.yml
│           ├── ai-pr-notify.yml
│           └── ai-security-scan.yml
│
├── platform-pipeline/           ← Team Platform (pipeline infrastrutturali)
│   └── .github/
│       ├── workflows/
│       │   └── arc-runner-update.yml
│       ├── arc-runner-dotnet/
│       │   ├── Dockerfile
│       │   └── values.yaml
│       └── arc-runner-node/
│           ├── Dockerfile
│           └── values.yaml
│
├── test-runner/                 ← Team Development (pipeline applicativa)
│   └── .github/
│       └── workflows/
│           ├── pipeline.yml                ← Richiama i Reusable Workflows centralizzati
│           ├── ai-enhanced-pipeline.yml
│           ├── ai-pr-notify.yml
│           ├── ai-summary.yml
│           └── ai.yml
│
├── storefront-app/
│   └── .github/
│       └── workflows/
│           ├── pipeline.yml
│           ├── ai-enhanced-pipeline.yml
│           ├── ai-pr-notify.yml
│           ├── ai-summary.yml
│           └── ai.yml
│
└── ...
```

## Key Components

### Platform Workflows
Reusable GitHub Actions workflows for common CI/CD patterns:
- **.NET builds** - ci-dotnet.yml
- **React Vite builds** - ci-react-vite.yml
- **Docker operations** - docker-build-push.yml
- **ARC deployments** - deploy-arc.yml
- **Tagging** - tag.yml

### Platform Actions
Composite reusable actions for modular pipeline steps:
- Copilot CLI setup and agent execution
- Kubernetes namespace creation
- ARC controller and cluster deployments
- Docker build and push operations
- Microsoft Teams notifications
- React Vite build automation
- Repository tagging

### Platform AI Workflows
AI-enhanced pipeline workflows:
- Daily summary generation
- Failure analysis and handling
- Pull request notifications
- Security scanning

### Platform Pipeline
Infrastructure-specific pipelines and ARC runner configurations for:
- .NET applications
- Node.js applications

### Application Pipelines
Individual application pipelines (test-runner, storefront-app) that reference centralized reusable workflows.
