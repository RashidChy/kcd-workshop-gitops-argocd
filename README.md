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

## Create a new Kubernetes cluster

We will use [kind](https://kind.sigs.k8s.io/) to create a new cluster on your local machine. Run the following command,

```
kind create cluster --name kcd-cluster-one
```

You can check the cluster context and use it.

```
kubectl config get-contexts

kubectl config use-context kind-kcd-cluster-one
```

## Install ArgoCD

For more info [click here](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd)

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Wait until all the required pods are running.

### Get ArgoCD Admin Password

```
kubectl get secrets/argocd-initial-admin-secret -n argocd --template={{.data.password}} | base64 -d
```

### ArgoCD Dashboard

Since we are working on local machine, we will be exposing the Argocd dashboard through port forwarding. In 
production, you can use kubernetes [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/#what-is-ingress) or 
[LoadBalancer](https://kubernetes.io/docs/concepts/services-networking/) to expose ArgoCD Web UI.

Run the following command to port-forward the argocd server. You can expose it to any port.

```
kubectl port-forward -n argocd svc/argocd-server 8080:80
```

Login with the username "admin" and password admin password from secret. 


## Install ArgoCD CLI

Install ArgoCD Cli tool to connect argocd from terminal. The installation guidelines for each operating system are 
documented [here](https://argo-cd.readthedocs.io/en/stable/cli_installation/)

## Login to ArgoCD from CLI

Since we are running our argocd in local machine using port forwarding, to connect with the argocd we need to
provide the localmachine IP, port and admin credentials. In production, you will need to provide the exposed ArgoCD URL for login along with
the admin credentials of ArgoCD.

Run the following command in your machine. **Make sure the port-forward is running, before executing the command**. To connect with the production ArgoCD, replace the server host and port with your
ArgoCD URL along with the admin username and password.

```
argocd login localhost:8080 --insecure \
--username admin \
--password $(kubectl get secrets/argocd-initial-admin-secret -n argocd --template={{.data.password}} | base64 -d)
```

Check if its successfully connected or not.

```
argocd cluster list
```

## Let's Get Started

### Task 1 - Deploy Application

Create a new application from ArgoCD UI and deploy the application. [Check here](https://github.com/shaekhhasanshoron/kcd-workshop-gitops-argocd/tree/main/manifests/demo-app) for the application descriptors.

https://github.com/shaekhhasanshoron/kcd-workshop-gitops-argocd

### Task 2 - Update the Image Deploy Application

Update the Image of the descriptors and Commit changes.


### Task 3 - On Board an Application using Helm

* Chart Repo - "https://charts.bitnami.com/bitnami"
* Chart Name - "wildfly"
* Target Version - "19.1.1"


### Task - Deploy application in another kubernetes cluster
#### Create a new Kubernetes cluster

```
kind create cluster --name kcd-cluster-two
```

#### Add new cluster to ArgoCD

Check the current cluster list

```
argocd cluster list
```

We will be using ArgoCD cli to attach new cluster. You need to check the current contexts to connect with a new cluster.

Run the command to check the cluster contexts.
```
kubectl config get-contexts
```

Now run the following command to attach a new cluster to argocd. Replace the context with actual context name.
```
argocd cluster add <context> --name <name> -y
```

Since we are using our local machines, the above `argocd cluster add` may not work properly because the argocd be 
able to connect to the new cluster using 127.0.0.1 IP. We need to change it inside the `kube cluster` file.

