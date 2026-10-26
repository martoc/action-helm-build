[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# action-helm-build

A GitHub Action that packages and pushes Helm charts to Docker Hub, AWS ECR, GCP Artifact Registry and the GitHub Container Registry (ghcr.io). It simplifies the CI/CD workflow by automating the process of versioning, packaging, and pushing Helm charts.

## Features

- Multi-registry support: Docker Hub, AWS ECR, GCP Artifact Registry, GitHub Container Registry
- Charts in any directory (`chart_path`) and an optional `appVersion`
- Automatic chart versioning from `TAG_VERSION`
- Helm chart packaging using `helm package`
- OCI registry push
- Workload Identity Federation support (GCP)
- IAM role assumption support (AWS)

## Quick Start

```yaml
- name: Build & Push Helm Chart (Docker Hub)
  uses: martoc/action-helm-build@v0
  with:
    registry: docker.io
    chart_path: charts/my-chart
    app_version: ${{ env.TAG_VERSION }}
  env:
    DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
    DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
```

```yaml
- name: Build & Push Helm Chart (AWS)
  uses: martoc/action-helm-build@v0
  with:
    registry: aws
    region: us-east-2
    repository_name: helm-charts
    aws_account_id: "123456789012"
```

```yaml
- name: Build & Push Helm Chart (GCP)
  uses: martoc/action-helm-build@v0
  with:
    registry: gcp
    region: europe-west2
    repository_name: helm-charts
    gcp_project_id: my-project
```

```yaml
# oci://ghcr.io/<owner>/charts/<chart>; the job needs permissions: packages: write
- name: Build & Push Helm Chart (GitHub)
  uses: martoc/action-helm-build@v0
  with:
    registry: github
    repository_name: ${{ github.repository_owner }}/charts
    chart_path: charts/my-chart
```

## Documentation

- [Usage Guide](./docs/USAGE.md) - Detailed usage instructions and examples
- [Code Style](./docs/CODESTYLE.md) - Code style guidelines for contributors

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `registry` | Registry to push to (`docker.io`, `gcp`, `aws`, `github`) | No | `docker.io` |
| `region` | GCP or AWS region | No | `""` |
| `repository_name` | Artifact Registry repository, ECR namespace, Docker Hub namespace (defaults to `DOCKER_USERNAME`) or `ghcr.io` namespace (defaults to the repository owner) | No | `""` |
| `aws_account_id` | AWS Account ID | No | `""` |
| `gcp_project_id` | Google Cloud Project ID | No | `""` |
| `chart_path` | Directory holding `Chart.yaml` | No | `.` |
| `app_version` | Sets `appVersion` when not empty | No | `""` |
| `github_token` | Token that pushes to `ghcr.io` (`github` registry) | No | the workflow's `GITHUB_TOKEN` |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `TAG_VERSION` | Version tag for the Helm chart (required) |
| `DOCKER_USERNAME` / `DOCKER_PASSWORD` | Docker Hub credentials (required for `docker.io`) |

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
