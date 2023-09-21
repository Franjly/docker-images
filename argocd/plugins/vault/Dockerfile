# https://argo-cd.readthedocs.io/en/stable/operator-manual/custom_tools/
FROM quay.io/argoproj/argocd:v2.8.3

# Set environment variables for tool versions
ARG ARGOCD_VAULT_PLUGIN_VERSION=1.16.1
ARG ARCH=arm64

# Switch to root for the ability to perform install
USER root

# Install tools needed for your repo-server to retrieve & decrypt secrets, render manifests 
# (e.g. curl, awscli, gpg, sops)
RUN apt-get update && \
    apt-get install -y \
        curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install ArgoCD Vault Plugin
RUN curl -LO "https://github.com/argoproj-labs/argocd-vault-plugin/releases/download/v${ARGOCD_VAULT_PLUGIN_VERSION}/argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_${ARCH}" && \
    chmod +x argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_${ARCH} && \
    mv argocd-vault-plugin_${ARGOCD_VAULT_PLUGIN_VERSION}_linux_${ARCH} /usr/local/bin/argocd-vault-plugin

# Switch back to non-root user
USER $ARGOCD_USER_ID