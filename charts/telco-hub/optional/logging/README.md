# Telco Hub Logging Chart

This Helm chart deploys an ArgoCD Application that manages logging configuration for the Telco Hub Pattern.

## Overview

The chart creates an ArgoCD Application that references logging configurations from the telco-hub repository path:
`telco-hub/configuration/reference-crs/optional/logging`

## Features

- **ArgoCD Application Management**: Deploys logging as a GitOps-managed application
- **Kustomize Patch Support**: Allows runtime customization of logging configurations
- **Flexible Sync Policies**: Configurable ArgoCD sync behavior
- **Component Enablement**: Can be conditionally enabled/disabled

## Configuration

### Component Enablement

You can enable or disable the logging component:

```yaml
telcoHub:
  components:
    logging:
      enabled: true  # Set to false to disable logging deployment
```

When `enabled: false`, the ArgoCD Application for logging will not be deployed.

### Basic Configuration

The chart uses values from the parent `telcoHub` configuration:

```yaml
telcoHub:
  git:
    repoURL: https://github.com/openshift-kni/telco-reference.git
    targetRevision: main
```

### Kustomize Patches

You can apply runtime patches to logging configurations:

```yaml
telcoHub:
  logging:
    kustomizePatches:
      - target:
          group: operators.coreos.com
          version: v1alpha1
          kind: Subscription
          name: cluster-logging
        patch: |-
          - op: replace
            path: "/spec/source"
            value: "redhat-operators"
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
   helm install telco-logging charts/telco-hub/logging \
     --namespace openshift-gitops \
     --create-namespace
   ```

2. **Check the ArgoCD Application:**

   ```bash
   oc get applications -n openshift-gitops logging
   ```

3. **Monitor sync status:**

   ```bash
   oc describe application logging -n openshift-gitops
   ```

4. **View logs:**

   ```bash
   oc logs -l app.kubernetes.io/name=argocd-application-controller -n openshift-gitops
   ```

### Cleanup Testing Deployment

```bash
helm uninstall telco-logging -n openshift-gitops
```

## Prerequisites

- OpenShift GitOps (ArgoCD) deployed
- `infra` ArgoCD project configured
- Access to telco-hub configuration repository

## Deployment

This chart is automatically deployed when included in the `values-hub.yaml` applications section:

```yaml
applications:
  telco-hub-logging:
    name: telco-hub-logging
    namespace: openshift-gitops
    project: infra
    path: charts/telco-hub/logging
```
