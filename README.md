# Telco Hub Pattern

> **A GitOps-based validated pattern for deploying and managing Telco Hub infrastructure using Red Hat OpenShift, Advanced Cluster Management (ACM), and Zero Touch Provisioning (ZTP).**

[![OpenShift](https://img.shields.io/badge/OpenShift-4.14+-red?logo=redhat)](https://www.redhat.com/en/technologies/cloud-computing/openshift)
[![Validated Patterns](https://img.shields.io/badge/Validated-Patterns-blue)](https://validatedpatterns.io/)
[![Helm](https://img.shields.io/badge/Helm-3.8+-blue?logo=helm)](https://helm.sh/)
[![ArgoCD](https://img.shields.io/badge/ArgoCD-GitOps-green?logo=argo)](https://argoproj.github.io/cd/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

[Live build status](https://validatedpatterns.io/ci/?pattern=telco-hub)

---

## 🎯 Overview

The **Telco Hub Pattern** delivers a production-ready, GitOps-based solution for deploying and managing telecommunications hub infrastructure. Built on the [Red Hat Validated Patterns](https://validatedpatterns.io/) framework, this pattern provides a modular approach to telco hub deployment with component-based enablement.

### Key Features

- **🎛️ Component-Based Architecture**: Individual Helm charts for each component with enable/disable controls
- **🚀 GitOps-Native**: Fully automated deployment via ArgoCD with integrated patterns framework
- **🔄 Lifecycle Management**: Integrated cluster management and upgrade capabilities via TALM
- **📋 Kustomize Patches**: Runtime customization without modifying [reference-crs](https://github.com/openshift-kni/telco-reference/tree/main/telco-hub/configuration/reference-crs) configurations
- **🔒 Zero Touch Provisioning**: Automated cluster installation and configuration workflows
- **📊 Observability Ready**: Built-in monitoring and logging options

---

## 🚀 Quick Start

### Prerequisites

- ✅ **OpenShift 4.14+** cluster with cluster-admin privileges
- ✅ **Git repository** access for telco-reference configurations
- ✅ **oc CLI** tool configured and authenticated

### Deploy

```bash
# Clone the repository
git clone https://github.com/leo8a/telco-hub-pattern.git
cd telco-hub-pattern

# Deploy using the pattern framework
./pattern.sh make install
```

### Verify

```bash
# Check pattern deployment status
./pattern.sh make argo-healthcheck

# Monitor ArgoCD applications
oc get applications.argoproj.io -n openshift-gitops
```

**🎉 Your Telco Hub is now deploying via GitOps!**

---

## 📦 Components

| Component | Type | Description | Chart Path |
|-----------|------|-------------|------------|
| **ACM** | Required | Advanced Cluster Management for multi-cluster operations | [`charts/telco-hub/required/acm/`](charts/telco-hub/required/acm/) |
| **GitOps** | Required | ArgoCD operators and GitOps configuration | [`charts/telco-hub/required/gitops/`](charts/telco-hub/required/gitops/) |
| **TALM** | Required | Topology Aware Lifecycle Manager for cluster updates | [`charts/telco-hub/required/talm/`](charts/telco-hub/required/talm/) |
| **ZTP Installation** | Workflow | Zero Touch Provisioning cluster installation workflow | Enabled via [GitOps chart](charts/telco-hub/required/gitops/) |
| **LSO** | Optional | Local Storage Operator for node-local storage | [`charts/telco-hub/optional/lso/`](charts/telco-hub/optional/lso/) |
| **ODF** | Optional | OpenShift Data Foundation for persistent storage | [`charts/telco-hub/optional/odf/`](charts/telco-hub/optional/odf/) |
| **Backup Recovery** | Optional | OADP for backup and disaster recovery | [`charts/telco-hub/optional/backup-recovery/`](charts/telco-hub/optional/backup-recovery/) |
| **Logging** | Optional | Cluster Logging Operator for log aggregation | [`charts/telco-hub/optional/logging/`](charts/telco-hub/optional/logging/) |

---

## Rationale

The goal for this pattern is to:

* Use a GitOps approach to manage telco-hub configurations on OpenShift hub clusters
* Provide component-based deployment with granular control over GitOps ZTP components
* Demonstrate integration with telco-reference configurations via kustomize patches
* Support Zero Touch Provisioning workflows for automated cluster deployment
* Deliver a foundation for building GitOps-based telco applications and network functions

This pattern is designed specifically for telecommunications use cases and provides a hub cluster configuration optimized for telco workloads, network functions, and operational requirements with enterprise-grade lifecycle management.

---

## 📚 Documentation

**📖 [Complete Getting Started Guide](docs/getting-started.md)** - Comprehensive setup, configuration, and operations guide

### External Resources

- [Red Hat Validated Patterns](https://validatedpatterns.io/learn)
- [OpenShift GitOps](https://docs.redhat.com/en/documentation/red_hat_openshift_gitops/latest/html/understanding_openshift_gitops/about-redhat-openshift-gitops)
- [Advanced Cluster Management](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/)
- [Zero Touch Provisioning](https://docs.openshift.com/container-platform/latest/scalability_and_performance/ztp_far_edge/ztp-deploying-far-edge-clusters-at-scale.html)
