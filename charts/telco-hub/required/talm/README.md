# Telco Hub TALM Chart

This Helm chart deploys an ArgoCD Application that manages TALM (Topology Aware Lifecycle Manager) configuration for the Telco Hub Pattern.

## Overview

The chart creates an ArgoCD Application that references TALM configurations from the telco-hub repository path:
`telco-hub/configuration/reference-crs/required/talm`

## Features

- **ArgoCD Application Management**: Deploys TALM as a GitOps-managed application
- **Kustomize Patch Support**: Allows runtime customization of TALM configurations
- **Flexible Sync Policies**: Configurable ArgoCD sync behavior
- **Component Enablement**: Can be conditionally enabled/disabled

## Configuration

### Component Enablement

You can enable or disable the TALM component:

```yaml
telcoHub:
  components:
    talm:
      enabled: true  # Set to false to disable TALM deployment
```

When `enabled: false`, the ArgoCD Application for TALM will not be deployed.

### Basic Configuration

The chart uses values from the parent `telcoHub` configuration:

```yaml
telcoHub:
  git:
    repoURL: https://github.com/openshift-kni/telco-reference.git
    targetRevision: main
```

### Kustomize Patches

You can apply runtime patches to TALM configurations:

```yaml
telcoHub:
  talm:
    kustomizePatches:
      - target:
          group: operators.coreos.com
          version: v1alpha1
          kind: Subscription
          name: topology-aware-lifecycle-manager
        patch: |-
          - op: replace
            path: "/spec/source"
            value: "redhat-operators"
          - op: replace
            path: "/spec/channel"
            value: "stable"
```

### Sync Policy

Customize ArgoCD sync behavior:

```yaml
telcoHub:
  argocd:
    syncPolicy:
      automated:
        allowEmpty: true
        prune: true
        selfHeal: true
```

## Quick Testing

For quick testing and development, you can deploy this chart standalone:

### Prerequisites

- OpenShift cluster with GitOps operator installed
- `oc` CLI tool configured

### Deploy for Testing

1. **Install the chart directly:**

   ```bash
   helm install telco-talm charts/telco-hub/talm \
     --namespace openshift-gitops \
     --create-namespace
   ```

2. **Check the ArgoCD Application:**

   ```bash
   oc get applications -n openshift-gitops talm
   ```

3. **Monitor sync status:**

   ```bash
   oc describe application talm -n openshift-gitops
   ```

4. **View logs:**

   ```bash
   oc logs -l app.kubernetes.io/name=argocd-application-controller -n openshift-gitops
   ```

### Cleanup Testing Deployment

```bash
helm uninstall telco-talm -n openshift-gitops
```

## Prerequisites

- OpenShift GitOps (ArgoCD) deployed
- `infra` ArgoCD project configured
- Access to telco-hub configuration repository

## Deployment

This chart is automatically deployed when included in the `values-hub.yaml` applications section:

```yaml
applications:
  talm:
    name: talm
    namespace: openshift-gitops
    project: infra
    path: charts/telco-hub/talm
```

## About TALM

TALM (Topology Aware Lifecycle Manager) is an operator that manages the lifecycle of clusters in edge computing environments. It provides:

- **Cluster Updates**: Orchestrates OpenShift cluster updates
- **Policy Management**: Applies configuration policies to clusters
- **Rollout Control**: Controls the rollout of updates across multiple clusters
- **Backup and Recovery**: Manages cluster backup and recovery operations
