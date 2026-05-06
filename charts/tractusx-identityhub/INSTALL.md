# Tractus-IdentityHub Installation Guide

This guide walks you through installing the Connector using Helm in a Kubernetes environment.

---

## 1. Prerequisites

Ensure the following tools are installed and configured:

- Kubernetes cluster (accessible via [`kubectl`](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/))
- Kuberneters Cluster access via kubeconfig
- [Helm](https://helm.sh/docs/intro/install/)
- [Git](https://git-scm.com/install/linux)

---

## 2. Clone the Repository

```shell
git clone https://github.com/project-construct-x/tractusx-identityhub.git
cd tractusx-identityhub

# Use feature branch until merged into develop/main
git checkout feat/helm-chart-config-workshop
```
## 3. Chart Structure and Dependencies
```
charts/
└── tractusx-identityhub/                              # Connector with persistence
    ├── Chart.yaml
    ├── values.yaml                                  # Default configuration
    ├── values-consumer.yaml                         # Consumer configuration
    ├── values-dev-ihub-consumer-grp-X.yaml         # Consumer specific group-X (1-5)
    ├── values-provider.yaml                         # Provider configuration
    ├── values-dev-ihub-provider-grp-X.yaml         # Provider specific group-X (1-5)
```

>Note: tractusx-identityhub-memory is not in scope (no persistence).


## 4. Build Chart Dependencies

```shell
cd charts/tractusx-identityhub
helm dependency update
```

## 5. Configure the IdentityHub Helm Chart

The identityhub configuraion has already been prepared for the workshop.


## 6. Install the IdentityHub

```shell
helm install consumer-idhub-grp-X \
  -n user-grp-X \
  -f values-dev-ihub-consumer-user-grp-X.yaml  \
  .
```

>Note: Please replace the placeholder `X` with your assigned group.

### Parameters

- `consumer-idhub-grp-X` → Release name
- `user-grp-X` → Kubernetes namespace
- `values-dev-ihub-consumer-grp-X.yaml` → Your Configuration file

## 7. Verify Installation

Expected output:


```shell
Release "consumer-idhub-grp-X" has been upgraded. Happy Helming!
NAME: consumer-idhub-grp-X
LAST DEPLOYED: Wed Apr 29 14:32:53 2026
NAMESPACE: user-grp-X
STATUS: deployed
REVISION: 1
...
```
Additional checks:
```shell
helm list -n user-grp-X
kubectl get pods -n user-grp-X
```

## 8 (Optional) Data Exchange

For the e2e data exchange journey, please refer to the [bruno](../../docs/usage/dcp-api-walkthrough/CX-IdentityHub/) collection


### Helm to manage Kubernetes

#### Basic Helm tricks

<details><summary>show</summary>
<p>

```shell
# Creating basic helm chart
helm create <CHART_NAME>

# Building chart dependencies
 helm dependency build <SOURCE>

# Updating chart dependencies
 helm dependency update <SOURCE>

# Installing helm release
helm install <CHART_NAME> -f myvalues.yaml ./SOURCE

# Uninstalling helm release
helm uninstall <CHART_NAME>

# Listing helm releases
helm list
```
<p>
</details>

#### Using Helm Repository
<details><summary>show</summary>
<p>

```shell
helm repo add [NAME] [URL]  [flags]

helm repo list / helm repo ls

helm repo remove [REPO1] [flags]

helm repo update / helm repo up

helm repo update [REPO1] [flags]

helm repo index [DIR] [flags]
```
<p>
</details>

#### Download a Helm chart from a repository 

<details><summary>show</summary>
<p>

```shell
helm pull [chart URL | repo/chartname] [...] [flags] ## this would download a helm, not install 
helm pull --untar [rep/chartname] # untar the chart after downloading it 
```
