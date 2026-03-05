[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# action-helm-build

A GitHub Action that packages and pushes Helm charts to AWS ECR and GCP Artifact Registry. It simplifies the CI/CD workflow by automating the process of versioning, packaging, and pushing Helm charts.

## Features

- Multi-registry support: AWS ECR, GCP Artifact Registry
- Automatic chart versioning from `TAG_VERSION`
- Helm chart packaging using `helm package`
- OCI registry push
- Workload Identity Federation support (GCP)
- IAM role assumption support (AWS)

## Quick Start

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

## Documentation

- [Usage Guide](./docs/USAGE.md) - Detailed usage instructions and examples
- [Code Style](./docs/CODESTYLE.md) - Code style guidelines for contributors

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `registry` | Registry to push to (`gcp`, `aws`) | No | `docker.io` |
| `region` | GCP or AWS region | No | `""` |
| `repository_name` | Repository name | No | `""` |
| `aws_account_id` | AWS Account ID | No | `""` |
| `gcp_project_id` | Google Cloud Project ID | No | `""` |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `TAG_VERSION` | Version tag for the Helm chart (required) |

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
