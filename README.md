# GitHub Actions Toolkit

Battle-tested, reusable GitHub Actions workflows and composite actions for DevOps pipelines.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## Why This Exists

Stop copy-pasting CI/CD YAML across repos. This toolkit provides production-ready, composable building blocks.

## Workflows

| Workflow | Trigger | What It Does |
|----------|---------|--------------|
| `docker-build-push.yml` | `workflow_call` | Build, scan, and push Docker images to GHCR |
| `terraform-plan-apply.yml` | `workflow_call` | Terraform fmt, validate, plan, apply |
| `k8s-deploy.yml` | `workflow_call` | Deploy manifests to EKS/GKE with kubectl |
| `release.yml` | Tag push | Semantic release + changelog generation |
| `security-scan.yml` | PR + schedule | Trivy, CodeQL, dependency review |

## Composite Actions

| Action | Description |
|--------|-------------|
| `setup-toolchain` | Install Terraform, kubectl, helm, aws-cli |
| `docker-metadata` | Generate image tags and labels |
| `slack-notify` | Send deployment notifications to Slack |

## Usage

### Docker Build & Push

```yaml
# .github/workflows/deploy.yml
jobs:
  build:
    uses: almightymoon/github-actions-toolkit/.github/workflows/docker-build-push.yml@main
    with:
      image-name: my-app
      context: .
    secrets: inherit
```

### Terraform Plan & Apply

```yaml
jobs:
  infra:
    uses: almightymoon/github-actions-toolkit/.github/workflows/terraform-plan-apply.yml@main
    with:
      working-directory: environments/dev
      terraform-version: "1.7.0"
    secrets: inherit
```

### Setup Toolchain

```yaml
steps:
  - uses: almightymoon/github-actions-toolkit/actions/setup-toolchain@v1
    with:
      terraform: "1.7.0"
      kubectl: "1.29.0"
      helm: "3.14.0"
```

## Design Principles

1. **Composable** — Mix and match workflows via `workflow_call`
2. **Secure** — OIDC for cloud auth, no long-lived secrets
3. **Fast** — Layer caching, matrix builds, concurrency groups
4. **Observable** — Job summaries, PR comments with plan output

## Contributing

PRs welcome! See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT © [almightymoon](https://github.com/almightymoon)
