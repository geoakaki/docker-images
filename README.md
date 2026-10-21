# docker-images

Small CI job images, built for `linux/amd64` and `linux/arm64` and published to GitHub Container Registry.

| Image | Pull | What is inside |
|---|---|---|
| `buildkit` | `ghcr.io/geoakaki/docker-images/buildkit:latest` | `buildctl` (BuildKit client), aws-cli, yq, jq, Rancher CLI, git, bash |
| `rancher` | `ghcr.io/geoakaki/docker-images/rancher:latest` | Rancher CLI, kubectl, helm, yq, jq, GNU sed, envsubst, aws-cli, git, bash |
| `ansible` | `ghcr.io/geoakaki/docker-images/ansible:latest` | ansible, boto3, awscli, netaddr, openssh-client, rsync, sshpass, git |

Each image is also tagged with its main tool's version (`buildkit:v0.33.1`, `rancher:v2.15.2`, `ansible:14.5.0`).
Pin that tag where a pipeline must not change under you.

## Build

`.github/workflows/build.yml` builds an image when its folder changes on `main`; *Run workflow* rebuilds all of them.
To bump a tool, change the `ARG …_VERSION` default in the Dockerfile and the matching `version` in the workflow matrix.

Local build of one image:

```sh
docker buildx build --platform linux/amd64,linux/arm64 -t docker-images/rancher:dev rancher/
```

No credentials, proxies or private hosts belong in these images: they are public.
