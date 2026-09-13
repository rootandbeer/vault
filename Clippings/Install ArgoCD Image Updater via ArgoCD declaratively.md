---
categories:
  - "[[Clippings]]"
tags:
  - "clippings"
author:
  - "[[View all posts by Yuki Nakamura]]"
url: "https://yuki-nakamura.com/2023/07/08/install-argocd-image-updater-via-argocd-declaratively/"
created: 2026-09-07
published: 2023-07-07
topics:
---
![](https://yuki-nakamura.com/wp-content/uploads/2023/07/image-45.png)

ArgoCD Image Updater allocation

**ArgoCD Image Updater**

ArgoCD image updater can automate the updating of image tags in manifests. It will make your CI/CD process more efficient! In this article, I will explain how to install ArgoCD Image Updater via ArgoCD using [ArgoCD’s Declarative Setup](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#helm-chart-repositories). 　

## Preparation

Please install tools below.

<sup>[1]</sup> Here are my versions of each tool for reference.

```
$ minikube profile list
|----------|-----------|---------|--------------|------|---------|---------|-------|--------|
| Profile  | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes | Active |
|----------|-----------|---------|--------------|------|---------|---------|-------|--------|
| minikube | docker    | docker  | 192.168.49.2 | 8443 | v1.26.3 | Running |     1 | *      |
|----------|-----------|---------|--------------|------|---------|---------|-------|--------|
 
$ argocd version --short | grep argocd-server
argocd-server: v2.7.4+a33baa3.dirty
 
$ helm version 
version.BuildInfo{Version:"v3.12.0", GitCommit:"c9f554d75773799f72ceef38c51210f1842a1dea", GitTreeState:"clean", GoVersion:"go1.20.4"}
```

If you’d like to install ArgoCD Image Updater with GitOps way, please prepare below, too.

- (Optional) GitHub repository for manifests <sup>[2]</sup>
- (Optional) ArgoCD Application that maintains ArgoCD Applications by following [App of Apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern).<sup>[3]</sup>

<sup>[2]</sup> Please refer to [my previous post](https://yuki-nakamura.com/2023/06/24/gitops-for-managing-argocd/) and create a GitHub repository for manifests.

<sup>[3]</sup> If you’re not familiar with App of Apps pattern, please check [the ArgoCD document](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern) or [my previous post](https://yuki-nakamura.com/2023/06/24/gitops-for-managing-argocd/).

## Add argo’s chart repository

ArgoCD’s Helm Charts are maintained on [argoproj](https://github.com/argoproj) / **[argo-helm](https://github.com/argoproj/argo-helm)**. Also, they are hosted at [https://argoproj.github.io/argo-helm/](https://argoproj.github.io/argo-helm/). Let’s add it to your helm.

```
helm repo add argo https://argoproj.github.io/argo-helm
```

If you’ve already added it, please update it.

```
helm repo update argo
```

There are several charts in the repository. You can find argocd-image-updater’ chart by the following command.

```
helm search repo argo | grep argocd-image-updater
```

example:

```
helm search repo argo | grep argocd-image-updater
argo/argocd-image-updater       0.9.1           v0.12.2         A Helm chart for Argo CD Image Updater, a tool ...
```

## Check ArgoCD’s namespace

There are two methods for installing ArgoCD Image Updater.

1. [Installing as Kubernetes workload in Argo CD namespace](https://argocd-image-updater.readthedocs.io/en/stable/install/installation/#method-1-installing-as-kubernetes-workload-in-argo-cd-namespace)
2. [Connect using Argo CD API Server](https://argocd-image-updater.readthedocs.io/en/stable/install/installation/#method-2-connect-using-argo-cd-api-server)

This time, we will use 1 because it’s easier than 2.

You can check ArgoCD’s namespace by the following command.

```
kubectl get deploy -A | grep -e '^NAMESPACE' -e argocd-server
```

In my case, I installed ArgoCD in a namespace called *argocd*.

```
NAMESPACE              NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
argocd                 my-argocd-server                      1/1     1            1           17d
```

## Install ArgoCD Image Updater

## Check available charts’ versions and ArgoCD Image Updater’s versions

You can check available versions via **helm search** with **–versions option**.

```
helm search repo argo/argocd-image-updater --versions | head -5
```

example:

```
NAME                            CHART VERSION   APP VERSION     DESCRIPTION                                       
argo/argocd-image-updater       0.9.1           v0.12.2         A Helm chart for Argo CD Image Updater, a tool ...
argo/argocd-image-updater       0.9.0           v0.12.2         A Helm chart for Argo CD Image Updater, a tool ...
argo/argocd-image-updater       0.8.5           v0.12.2         A Helm chart for Argo CD Image Updater, a tool ...
argo/argocd-image-updater       0.8.4           v0.12.2         A Helm chart for Argo CD Image Updater, a tool ...
```

This time, I chose the latest version (0.9.1) that uses v0.12.2 ArgoCD Image Updater.

## Create Application manifest of ArgoCD Image Updater

Store the following in a file called *my-argocd-image-updater.yaml*. Please put your ArgoCD’s namespace in the *spec.destination.namespace* field.

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-argocd-image-updater
spec:
  destination:
    namespace: <ArgoCD's namespace>
    server: 'https://kubernetes.default.svc'
  source:
    path: ''
    repoURL: 'https://argoproj.github.io/argo-helm'
    targetRevision: 0.9.1
    chart: argocd-image-updater
    helm:
      parameters:
  sources: []
  project: default
  syncPolicy:
    automated:
      prune: false
      selfHeal: false
```

Apply the manifest to your kubernetes. Please put your ArgoCD’s namespace in the command.

```
kubectl apply -f my-argocd-image-updater.yaml -n <ArgoCD's namespace>
```

(Optional) If you’re managing Application’s manifests in GitHub, please commit and push it to the repository.

example:

```
git add argocd-resources/applications/my-argocd-image-updater.yaml \
&& git commit -m "Add my-argocd-image-updater" \
&& git push
```

## Check if it has been installed on ArgoCD UI.

Please sign in your ArgoCD UI, then check if ArgoCD Image Updater has been installed.

![](https://yuki-nakamura.com/wp-content/uploads/2023/07/image-43.png)

my-argocd-image-updater

Now you are ready to use the ArgoCD Image Updater!

## Test ArgoCD Image Updater

Let’s run *[test](https://argocd-image-updater.readthedocs.io/en/stable/install/testing/)* command to verify the behavior of Argo CD Image Updater on arbitrary images and to validate your configuration.

## Look up image updater’s pod name

Look up image updater’s pod name by the following command.

```
kubectl get po -n <ArgoCD's namespace> | grep -e '^NAME' -e 'image-updater'
```

example:

```
$ kubectl get po -n argocd | grep -e '^NAME' -e 'image-updater'
NAME READY STATUS RESTARTS AGE
my-argocd-image-updater-5f49876fc7-4mnxp 1/1 Running 1 (15m ago) 3d12h
```

## Run test command

Let’s check if it can access [the nginx repository on DockerHub](https://hub.docker.com/_/nginx) as a test.

```
kubectl exec <pod name> -n  <ArgoCD's namespace> -- argocd-image-updater test nginx
```

example:

```
kubectl exec my-argocd-image-updater-5f49876fc7-4mnxp -n argocd -- argocd-image-updater test nginx
time="2023-07-08T02:00:11Z" level=debug msg="Creating in-cluster Kubernetes client"
time="2023-07-08T02:00:11Z" level=info msg="retrieving information about image" image_alias= image_name=nginx registry_url=
time="2023-07-08T02:00:11Z" level=info msg="Fetching available tags and metadata from registry" application=test image_alias= image_name=nginx registry_url=
 
skip...
 
time="2023-07-08T02:00:14Z" level=info msg="latest image according to constraint is nginx:1.25.1" application=test image_alias= image_name=nginx registry_url=
```

## (Optional) Authentication to container registries

If you’d like to your private registries, you need to configure authentication. Please check the official documentations.

## Wrap up

We installed ArgoCD Image Updater. In a next article, I will share how to use it to automate the updating of image tags in manifests.

Thank you for reading!