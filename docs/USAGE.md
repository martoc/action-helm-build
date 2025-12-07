# Usage

## Overview

This GitHub Action is designed to package and push Helm charts to Google Cloud Artifact Registry.
It simplifies the CI/CD workflow by automating the process of versioning, packaging,
and pushing Helm charts.

## Prerequisites

Secrets Configuration

Before using this action, ensure that you have the following secrets configured in your GitHub repository:

### GCP Artifact Registry

* **GCP_WORKLOAD_IDENTITY_PROVIDER:** Workload identity provider for GCP authentication.
* **GCP_SERVICE_ACCOUNT:** The email of the service account to use for GCP authentication.

## Inputs

### Action Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `registry` | Registry to push the Helm chart to. Valid values: `gcp` | No | `docker.io` |
| `region` | Region to push the Helm chart to. Valid for GCP regions | No | `""` |
| `repository_name` | Repository name | No | `""` |
| `aws_account_id` | AWS Account ID (reserved for future use) | No | `""` |
| `gcp_project_id` | Google Cloud Project ID | No | `""` |

### Environment Variables

The following environment variables can be used to customize the build:

| Variable | Description | Required |
|----------|-------------|----------|
| `TAG_VERSION` | The version tag for the Helm chart | Yes |

## Example

Here are examples of how to use this GitHub Action to package and push Helm
charts to GCP Artifact Registry.

```yaml
name: Integration Tests

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  gcp:
    permissions:
      contents: write
      id-token: write
    runs-on: ubuntu-24.04
    steps:
      - uses: actions/checkout@v6
        with:
          fetch-depth: 50
          fetch-tags: true
      - name: Tag
        uses: martoc/action-tag@v0
        with:
          skip-push: true
      - uses: google-github-actions/auth@v2
        with:
          workload_identity_provider: ${{ secrets.GCP_WORKLOAD_IDENTITY_PROVIDER }}
          service_account: ${{ secrets.GCP_SERVICE_ACCOUNT }}
      - name: Build & Push Helm Chart
        uses: martoc/action-helm-build@v0
        with:
          registry: gcp
          region: europe-west2
          repository_name: repository
          gcp_project_id: project-id
```

## Registry-Specific Requirements

### GCP Artifact Registry

* `registry` input must be set to `gcp`
* `region` input is required
* `repository_name` input is required
* `gcp_project_id` input is required
* GCP authentication must be configured (e.g., using `google-github-actions/auth`)

## Chart Versioning

The action automatically updates the `version` field in `Chart.yaml` with the value from the `TAG_VERSION` environment variable before packaging the chart.
