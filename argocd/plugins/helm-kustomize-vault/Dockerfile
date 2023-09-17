# Define the ARCH environment variable (change to the desired architecture)
ARG ARCH=arm64
ARG GIT_VERSION=2.42.0

# Stage 1: Build a temporary image to download and extract kubectl, helm, and kustomize
FROM alpine/curl AS builder

# Set environment variables for binary versions (you can change these)
ARG KUBECTL_VERSION=1.28.2
ARG HELM_VERSION=3.12.3
ARG KUSTOMIZE_VERSION=5.1.1
ARG ARGOCD_VAULT_PLUGIN_VERSION=1.16.1
ARG GIT_VERSION
ARG ARCH

# Download kubectl, helm, kustomize, argocd-vault-plugin and git
RUN curl -fsSL "https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/${ARCH}/kubectl" -o /tmp/kubectl && \
    curl -fsSL "https://get.helm.sh/helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz" | tar -zxvf - -C /tmp/ && \
    curl -fsSL "https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv${KUSTOMIZE_VERSION}/kustomize_v${KUSTOMIZE_VERSION}_linux_${ARCH}.tar.gz" | tar -zxvf - -C /tmp/ && \
    curl -fsSL "https://github.com/argoproj-labs/argocd-vault-plugin/releases/download/v${ARGOCD_VAULT_PLUGIN_VERSION}/argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_${ARCH}" -o /tmp/argocd-vault-plugin && \
    curl -fsSL "https://github.com/git/git/archive/refs/tags/v${GIT_VERSION}.tar.gz" | tar -zxvf - -C /tmp/

# Stage 2: Build the final image with busybox and the extracted binaries
FROM busybox

ARG GIT_VERSION
ARG ARCH

# Copy kubectl, helm, kustomize and argocd-vault-plugin from the builder stage
COPY --from=builder /tmp/kubectl /usr/local/bin/kubectl
COPY --from=builder /tmp/linux-${ARCH}/helm /usr/local/bin/helm
COPY --from=builder /tmp/kustomize /usr/local/bin/kustomize
COPY --from=builder /tmp/argocd-vault-plugin /usr/local/bin/argocd-vault-plugin

# Set permissions to make the binaries executable
RUN chmod +x /usr/local/bin/kubectl /usr/local/bin/helm /usr/local/bin/kustomize /usr/local/bin/argocd-vault-plugin

# Install Git

RUN cd /tmp/git-${GIT_VERSION} && \
    make prefix=/usr/local all && \
    make prefix=/usr/local install

# Clean up
RUN rm -rf /tmp/git*