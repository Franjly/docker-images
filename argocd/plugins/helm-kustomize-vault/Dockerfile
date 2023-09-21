# Use Alpine Linux as the base image
FROM alpine:latest

# Set environment variables for tool versions
ARG KUBECTL_VERSION=1.28.2
ARG HELM_VERSION=3.12.3
ARG KUSTOMIZE_VERSION=5.1.1
ARG ARGOCD_VAULT_PLUGIN_VERSION=1.16.1
ARG GIT_VERSION=2.42.0
ARG ARCH=arm64

# Install required packages
RUN apk add --update --no-cache \
    curl \
    tar \
    gzip \
    bash

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/v${KUBECTL_VERSION}/bin/linux/${ARCH}/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/

# Install Helm
RUN curl -LO "https://get.helm.sh/helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz" && \
    tar -zxvf "helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz" && \
    mv linux-${ARCH}/helm /usr/local/bin/ && \
    rm -rf linux-${ARCH}

# Install Kustomize
RUN curl -LO "https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv${KUSTOMIZE_VERSION}/kustomize_v${KUSTOMIZE_VERSION}_linux_${ARCH}.tar.gz" && \
    tar -zxvf "kustomize_v${KUSTOMIZE_VERSION}_linux_${ARCH}.tar.gz" && \
    chmod +x kustomize && \
    mv kustomize /usr/local/bin/kustomize

# Install ArgoCD Vault Plugin
RUN curl -LO "https://github.com/argoproj-labs/argocd-vault-plugin/releases/download/v${ARGOCD_VAULT_PLUGIN_VERSION}/argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_${ARCH}" && \
    chmod +x argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_${ARCH} && \
    mv argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_${ARCH} /usr/local/bin/argocd-vault-plugin

# Install Git
RUN apk add --update --no-cache git

# Clean up
RUN rm -rf /var/cache/apk/*

# Set argocd plugin user id
USER 999

WORKDIR /home/argocd
