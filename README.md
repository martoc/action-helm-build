[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# action-helm-build

A GitHub Action that packages and pushes Helm charts to GCP Artifact Registry. It simplifies the CI/CD workflow by automating the process of versioning, packaging, and pushing Helm charts.

## Features

- Automatic chart versioning from `TAG_VERSION`
- Helm chart packaging using `helm package`
- OCI registry push to GCP Artifact Registry
- Workload Identity Federation support

## Quick Start

```yaml
- uses: google-github-actions/auth@v2
  with:
    workload_identity_provider: ${{ secrets.GCP_WORKLOAD_IDENTITY_PROVIDER }}
    service_account: ${{ secrets.GCP_SERVICE_ACCOUNT }}

- name: Build & Push Helm Chart
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
| `registry` | Registry to push to (`gcp`) | No | `docker.io` |
| `region` | GCP region | No | `""` |
| `repository_name` | Repository name | No | `""` |
| `gcp_project_id` | Google Cloud Project ID | No | `""` |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `TAG_VERSION` | Version tag for the Helm chart (required) |

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
