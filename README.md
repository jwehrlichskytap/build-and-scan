# build-and-scan
Build docker image and run Trivy scans

## Example
```yaml
name: Skytap Service Action
on:
  workflow_dispatch:
  push:
    branches:
      - master
    tags:
      - '*'

jobs:
  build:
    runs-on: [self-hosted, qa]

    steps:
      - name: Generate a token
        id: app_token
        uses: actions/create-github-app-token@v1
        with:
          app-id: ${{ vars.APP_ID }}
          private-key: ${{ secrets.APP_PRIVATE_KEY }}
          owner: ${{ github.repository_owner }}

      - name: Build & Scan Service
        uses: skytap/build-and-scan@main
        with:
          service_name: ${{ matrix.service_name }}
          github_token: ${{ steps.app_token.outputs.token }}
          private_docker_registry: docker.prod.skytap.com:5000
          docker_tag: docker.prod.skytap.com:5000/skytap/<skytap-service>:${{ github.sha }}
```
