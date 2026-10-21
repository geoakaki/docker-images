# CI job image for deploys through Rancher: rancher CLI (its kubectl proxy), kubectl, helm, and the
# text tools the manifest render chain uses (yq, jq, GNU sed, envsubst).
FROM alpine:3.22

# Set by buildx; empty with the legacy builder, so each RUN falls back to uname -m.
ARG TARGETARCH
ARG KUBECTL_VERSION=v1.36.4
ARG RANCHER_CLI_VERSION=v2.15.2
ARG HELM_VERSION=v4.3.1
ARG YQ_VERSION=v4.54.1

RUN apk add --no-cache \
      aws-cli \
      bash \
      ca-certificates \
      curl \
      gettext \
      git \
      jq \
      sed \
      zip

RUN A="${TARGETARCH:-$(uname -m | sed -e s/x86_64/amd64/ -e s/aarch64/arm64/)}" \
 && curl -fsSL -o /usr/local/bin/kubectl \
      "https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/${A}/kubectl" \
 && chmod +x /usr/local/bin/kubectl

RUN A="${TARGETARCH:-$(uname -m | sed -e s/x86_64/amd64/ -e s/aarch64/arm64/)}" \
 && curl -fsSL "https://github.com/rancher/cli/releases/download/${RANCHER_CLI_VERSION}/rancher-linux-${A}-${RANCHER_CLI_VERSION}.tar.gz" \
      | tar -xz -C /tmp \
 && mv "/tmp/rancher-${RANCHER_CLI_VERSION}/rancher" /usr/local/bin/rancher \
 && rm -rf /tmp/rancher-*

RUN A="${TARGETARCH:-$(uname -m | sed -e s/x86_64/amd64/ -e s/aarch64/arm64/)}" \
 && curl -fsSL "https://get.helm.sh/helm-${HELM_VERSION}-linux-${A}.tar.gz" \
      | tar -xz -C /tmp \
 && mv "/tmp/linux-${A}/helm" /usr/local/bin/helm \
 && rm -rf "/tmp/linux-${A}"

RUN A="${TARGETARCH:-$(uname -m | sed -e s/x86_64/amd64/ -e s/aarch64/arm64/)}" \
 && curl -fsSL -o /usr/local/bin/yq \
      "https://github.com/mikefarah/yq/releases/download/${YQ_VERSION}/yq_linux_${A}" \
 && chmod +x /usr/local/bin/yq

RUN kubectl version --client && rancher --version && helm version --short && yq --version && aws --version

CMD ["bash"]
