# Usage

## Overview

This GitHub Action is designed to package and push Helm charts to AWS Elastic Container Registry (ECR) and Google Cloud Artifact Registry.
It simplifies the CI/CD workflow by automating the process of versioning, packaging,
and pushing Helm charts.

## Prerequisites

Secrets Configuration

Before using this action, ensure that you have the following secrets configured in your GitHub repository:

### AWS ECR

* **AWS_ROLE_TO_ASSUME:** The ARN of the AWS role to assume for authentication.

### GCP Artifact Registry

* **GCP_WORKLOAD_IDENTITY_PROVIDER:** Workload identity provider for GCP authentication.
* **GCP_SERVICE_ACCOUNT:** The email of the service account to use for GCP authentication.

## Inputs

### Action Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `registry` | Registry to push the Helm chart to. Valid values: `gcp`, `aws` | No | `docker.io` |
| `region` | Region to push the Helm chart to. Valid for GCP or AWS regions | No | `""` |
| `repository_name` | Repository name | No | `""` |
| `aws_account_id` | AWS Account ID | No | `""` |
| `gcp_project_id` | Google Cloud Project ID | No | `""` |

### Environment Variables

The following environment variables can be used to customize the build:

| Variable | Description | Required |
|----------|-------------|----------|
| `TAG_VERSION` | The version tag for the Helm chart | Yes |

## Example

Here are examples of how to use this GitHub Action to package and push Helm
charts to AWS ECR and GCP Artifact Registry.

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
  aws:
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
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_TO_ASSUME }}
          role-session-name: github-actions
          aws-region: us-east-2
      - name: Build & Push Helm Chart
        uses: martoc/action-helm-build@v0
        with:
          registry: aws
          region: us-east-2
          repository_name: helm-charts
          aws_account_id: "123456789012"

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

### AWS ECR

* `registry` input must be set to `aws`
* `region` input is required
* `repository_name` input is required
* `aws_account_id` input is required
* AWS credentials must be configured (e.g., using `aws-actions/configure-aws-credentials`)

### GCP Artifact Registry

* `registry` input must be set to `gcp`
* `region` input is required
* `repository_name` input is required
* `gcp_project_id` input is required
* GCP authentication must be configured (e.g., using `google-github-actions/auth`)

## Chart Versioning

The action automatically updates the `version` field in `Chart.yaml` with the value from the `TAG_VERSION` environment variable before packaging the chart.
