# VSCodium Linux Build Agents
![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/vscodium/vscode-linux-build-agent/build.yml)

Fork of [microsoft/vscode-linux-build-agent](https://github.com/microsoft/vscode-linux-build-agent), but pushes to [Docker Hub](https://hub.docker.com/repository/docker/vscodium/vscodium-linux-build-agent) instead of ACR. Also added `jq` since VSCodium uses it to edit the product.json at build time. Will be trying to stay in sync with upstream manually.

## License
MIT
