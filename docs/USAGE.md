# Usage

* Push a Helm chart to a Google Cloud Artifactory Registry

```yaml
- name: Build
  uses: martoc/action-helm-build@v0
  with:
    registry: gcp
    region: europe-west2
    repository_name: repository-name
    gcp_project_id: project-id
```
