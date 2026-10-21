# CI job image for container builds: the buildctl client plus the tools the build scripts call
# (registry login, repository create, manifest edits). The BuildKit daemon itself runs elsewhere
# (a service container or a remote buildkitd, reached through $BUILDKIT_HOST).
ARG BUILDKIT_VERSION=v0.33.1

FROM moby/buildkit:${BUILDKIT_VERSION}

# Set by buildx; empty with the legacy builder, so each RUN falls back to uname -m.
ARG TARGETARCH
ARG YQ_VERSION=v4.54.1
ARG RANCHER_CLI_VERSION=v2.15.2

USER root

RUN apk add --no-cache \
      aws-cli \
      bash \
      ca-certificates \
      coreutils \
      curl \
      git \
      jq

RUN A="${TARGETARCH:-$(uname -m | sed -e s/x86_64/amd64/ -e s/aarch64/arm64/)}" \
 && curl -fsSL -o /usr/local/bin/yq \
      "https://github.com/mikefarah/yq/releases/download/${YQ_VERSION}/yq_linux_${A}" \
 && chmod +x /usr/local/bin/yq

RUN A="${TARGETARCH:-$(uname -m | sed -e s/x86_64/amd64/ -e s/aarch64/arm64/)}" \
 && curl -fsSL "https://github.com/rancher/cli/releases/download/${RANCHER_CLI_VERSION}/rancher-linux-${A}-${RANCHER_CLI_VERSION}.tar.gz" \
      | tar -xz -C /tmp \
 && mv "/tmp/rancher-${RANCHER_CLI_VERSION}/rancher" /usr/local/bin/rancher \
 && rm -rf /tmp/rancher-*

RUN buildctl --version && aws --version && yq --version && rancher --version

# CI drives everything through script:, so no daemon entrypoint here.
ENTRYPOINT []
CMD ["bash"]
