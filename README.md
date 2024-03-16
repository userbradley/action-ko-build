# GitHub Action: ko Build

KO Build action to build Go Containers without the grim Dockerfile

## Quickstart

```yaml
name: "KO: Do the thing"
on:
  push:
    branches:
      - main
      - feature/**
    paths:
      - .github/workflows/ko-do-the-thing.yaml
      - containers/example/**
jobs:
  docker-build:
    name: Docker Build
    runs-on: ubuntu-latest
    timeout-minutes: 5
    permissions:
      id-token: write
      contents: read
    steps:
      - name: Checkout Repo
        uses: actions/checkout@v4
      - name: Authenticate to Google
        uses: google-github-actions/auth@v2
        with:
          project_id: 'my-project'
          workload_identity_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
      - name: Setup Go environment
        uses: actions/setup-go@v5.0.0
        with:
          go-version: 1.21.4
          cache-dependency-path: |
            containers/do-the-thing/go.sum
      - name: Ko build
        uses: userbradley/action-ko-build@v0.0.2
        with:
          repository: example
          googleProject: example-project
          image: do-the-thing
          directory: containers/do-the-thing
```
## Inputs

| Name            | Description                                              | Required | Default Value |
|-----------------|----------------------------------------------------------|----------|---------------|
| `repository`    | Name of the Artifact Registry Repository                 | `true`   | `Null`        |
| `googleProject` | Name of the google artifact registry project             | `true`   | `Null`        |
| `image`         | Name of the Image to build                               | `true`   | `Null`        |
| `directory`     | Location of the directory to build the docker image from | `true`   | `Null`        |
