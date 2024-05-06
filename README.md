# Workshop On Deploy Application in Kubernetes Using GitOps & ArgoCD

Hello everyone, welcome to the workshop on "Deploy Application in Kubernetes Using GitOps & [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)". 

In this workshop we will be working on the following tasks.

Task 1: Deploy an application in your k8 cluster using GitOps and [ArgoCD](https://argo-cd.readthedocs.io/en/stable/).

Task 2: Deploy a helm chart in your k8 cluster using GitOps and [ArgoCD](https://argo-cd.readthedocs.io/en/stable/).

Along with these tasks, we will be seeing different functionalities, scopes and best practices of [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) and GitOps.

## Pre-Requisite

To follow along with this workshop, you will require a basic understanding of the followings:

1. Basic knowledge of [Kubernetes](https://kubernetes.io/) and its different resources and components (such as: Deployments, Pods, Services etc).
2. Basic understanding of [Helm](https://helm.sh/).
3. Basic idea of [yaml](https://yaml.org/).
4. [Kubernetes manifests file](https://spacelift.io/blog/kubernetes-manifest-file#what-is-a-kubernetes-manifest-file).
5. Basic usage of [Kubectl](https://kubernetes.io/docs/tasks/tools/).
6. Basic understanding of [Docker](https://docker.io/). 

If you full-fill all the requirements, then you are good to go.

## Setup Environment

Let's start with setting our environment. To start working with [ArgoCD](https://argo-cd.readthedocs.io/en/stable/). 

For this workshop we will be working on our local machine. We'll setup [Kubernetes](https://kubernetes.io/) cluster using [Kind](https://kind.sigs.k8s.io/) deploy [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) on our machine. 

For production environment, you will need to setup [Kubernetes](https://kubernetes.io/) production cluster. [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) setup is same for any environment.

So, before proceeding further, let's setup the environment. To setup environment for the workshop, your local machine should have the following tools pre installed.

* [Docker](https://docker.io/)
* [Kind](https://kind.sigs.k8s.io/)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/)

## Install ArgoCD

For more info [click here](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd)

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Check ArgoCD Admin Password

```
kubectl get secrets/argocd-initial-admin-secret -n argocd --template={{.data.password}} | base64 -d
```

### ArgoCD Port Forward

```
kubectl port-forward -n argocd svc/argocd-server 8080:80
```


## Get Started

### Task 1 - Deploy Application

Create a new application from ArgoCD UI and deploy the application. [Check here](https://github.com/shaekhhasanshoron/kcd-workshop-gitops-argocd/tree/main/manifests/demo-app) for the application descriptors.

### Task 2 - Update the Image Deploy Application

Update the Image of the descriptors and Commit changes.


### Task 3 - On Board an Application using Helm

* Chart Repo - "https://charts.bitnami.com/bitnami"
* Chart Name - "wildfly"
* Target Version - "19.1.1"

----------------------------------------------------------------



* Chart Repo - "https://bitnami-labs.github.io/sealed-secrets"
* Chart Name - "sealed-secrets"
* Target Version - "1.16.1"

