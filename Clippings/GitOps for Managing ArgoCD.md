---
categories:
  - "[[Clippings]]"
tags:
  - "clippings"
author:
  - "[[View all posts by Yuki Nakamura]]"
url: "https://yuki-nakamura.com/2023/06/24/gitops-for-managing-argocd/"
created: 2026-09-07
published: 2023-06-23
topics:
---
**GitOps with ArgoCD**

In [my previous post](https://yuki-nakamura.com/2023/06/17/manage-argocd-by-argocd-declaratively-%f0%9f%90%99/), I explained how to create a manifest to manage ArgoCD declaratively. However, I only saved it on my local machine. That manifest should be shared with the team. Additionally, in the GitOps approach, the shared manifest and the state of ArgoCD should be in sync. So, in this post, I will explain how to introduce GitOps for managing ArgoCD using GitHub.

## Preparation

Please prepare the followings. If you haven’t installed ArgoCD yet, please install it referring to [my post](https://yuki-nakamura.com/2023/06/12/install-argocd-via-helm/).

- kubernetes
- ArgoCD
- GitHub account

Here are my versions of each tool for reference.

```
$ minikube profile list
|----------|-----------|---------|--------------|------|---------|---------|-------|--------|
| Profile  | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes | Active |
|----------|-----------|---------|--------------|------|---------|---------|-------|--------|
| minikube | docker    | docker  | 192.168.49.2 | 8443 | v1.26.3 | Running |     1 | *      |
|----------|-----------|---------|--------------|------|---------|---------|-------|--------|
$ argocd version --short | grep argocd-server
argocd-server: v2.7.4+a33baa3.dirty
```

## Create a repository at GitHub

Let’s create a repository for kubernetes manifests. Please access [*‘Create a new repository page’*](https://github.com/new) and create a new repository. No special settings are required. Here are my settings.

- Repository name: `my-manifests`
- Description: `Kubernetes Manifests for GitOps with ArgoCD`
- Accessibility: `Public`
- Checked `Add a README file`

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-31.png)

## Connect the Repository from ArgoCD

Next, let’s connect ArgoCD to the repository. ArgoCD offers [multiple methods](https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/#private-repositories) to access, but this time, we will use [Access Token authentication with HTTPS](https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/#access-token).

## Create a token of GitHub

Create a token that allow ArgoCD to access the repository. Please access [*‘Settings>Developer Settings>Personal access tokens>Fine-grained tokens’*](https://github.com/settings/tokens?type=beta) and click *‘Generate new token’.*

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-32.png)

Here are my settings.

- Token name: `Token for ArgoCD`
- Expiration: `30 days`
- Description: `This token will be used by ArgoCD`
- Repository access: `Only select repositories: repository=my-manifests`
- Permissions:
	- Repository permissions:
		- Commit statuses: `Read-only`
				- **Contents: `Read and write`**
				- Others: `default`
		- Account permissions: `default`

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-34.png)

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-37.png)

After you have finished creating a token, you can view it. Please make sure to keep a copy of that value in a secure location.

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-38.png)

## Connect the repository

Let’s connect ArgoCD to the repository. First, please sign in your ArgoCD.

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-39.png)

Access *‘Settings>Repositories’* and Click *‘CONNECT REPO’*.

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-40.png)

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-41.png)

Please fill in the fields as follows.

- connection method: `VIA HTTPS`
- Type: `git`
- Project: `default`
- Repository: \<YOUR REPOSITORY’s HTTPS URL>
	- e.g. `https://github.com/yukinakanaka/my-manifests.git`
- Username: \<YOUR GITHUB USERNAME>
- Password: \<THE TOKEN YOU’VE CREATED>

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-44.png)

Click *‘CONNECT’* on the top.

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-43.png)

Check if ‘ *CONNECTION STATUS* ‘ is **Successful**.

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-45.png)

## Upload a manifest to GitHub

Now, we are ready to do GitOps. Let’s upload a manifest to GitHub from your local machine.

## Pull the repository

```
git clone <YOUR REPOSITORY URL>
```

## Create directories for ArgoCD resources

```
cd <YOUR REPOSITORY NAME>
```

ArgoCD has [multiple types of resources](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#declarative-setup) such as Application and Project. Let’s make one directory per type to organize many manifests. This time, I will create one directory for **Application**.

```
mkdir -p argocd-resources/applications; cd argocd-resources/applications
```

## Commit and push Application manifest of ArgoCD

Create a Application manifest of ArgoCD in the directory. I will use the same manifest that [I created in the previous post.](https://yuki-nakamura.com/2023/06/17/manage-argocd-by-argocd-declaratively/)

```
cat << EOF > my-argocd.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-argocd
spec:
  destination:
    name: ''
    namespace: argocd
    server: 'https://kubernetes.default.svc'
  source:
    path: ''
    repoURL: 'https://argoproj.github.io/argo-helm'
    targetRevision: 5.36.1
    chart: argo-cd
    helm:
      parameters:
        - name: applicationSet.livenessProbe.failureThreshold
          value: '4'
  sources: []
  project: default
EOF
```

Commit the file and push it to GitHub

```
git add my-argocd.yaml \
&& git commit -m "Add Application manifest of ArgoCD" \
&& git push
```

## Create Application of ArgoCD

Let’s create an Application named argocd-resources that contains the Application named my-argocd. This approach is called [App of Apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern) because an Application has multiple Applications. (For now, there is only one Application, but we can add another Application in it easily.)

## Create Application named argocd-resources

Click *‘NEW APP’*

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-46.png)

## Fill in the form

**GENERAL**

- Application Name: `argocd-resources`
- Project Name: `default`
- SYNC POLICY: `Automatic`

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-47.png)

**SOURCE**

- Repository URL: \<YOUR REPOSITORY URL>
- Revision: `HEAD`
- Path: `argocd-resources`

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-48.png)

**DESTINATION**

- Cluster URL: `https://kubernetes.default.svc`
- Namespace: `argocd`

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-49.png)

**DIRECTORY**

- DIRECTORY RECURSE: put a check mark

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-50.png)

Click *‘CREATE’*

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-51.png)

Please check if argocd-resources has my-argocd. **==It is now possible to modify my-argocd’s configuration through GitHub’s manifest!==**

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-52.png)

## Practice GitOps!

It’s time to practice GitOps! In this scenario, let’s change replicas of argocd-server and syncPolicy of my-argocd by updating the manifest.

## Edit the manifest

Add [server.replicas parameter](https://github.com/argoproj/argo-helm/blob/490ffa574c0917def580efafa94e810e28ce02cb/charts/argo-cd/values.yaml#L1434) to the manifest and set its value to 2.

```
cat << EOF > my-argocd.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-argocd
spec:
  destination:
    name: ''
    namespace: argocd
    server: 'https://kubernetes.default.svc'
  source:
    path: ''
    repoURL: 'https://argoproj.github.io/argo-helm'
    targetRevision: 5.36.1
    chart: argo-cd
    helm:
      parameters:
        - name: applicationSet.livenessProbe.failureThreshold
          value: '4'
        - name: server.replicas
          value: '2'
  sources: []
  project: default
EOF
```

## Commit and push it

```
git add my-argocd.yaml \
&& git commit -m "Set server.replicas=2" \
&& git push
```

## Check argocd-resources

We set argocd-resources’ SYNC POLICY as Automatic, so ArgoCD will sync the manifest and Application automatically. [The automatic sync interval is 3 minutes by default](https://argo-cd.readthedocs.io/en/stable/user-guide/auto_sync/#automated-sync-semantics). So, please wait a moment. It is OK if you can see the new Comment int the *LAST SYNC*!

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-53.png)

## Check my-argocd

Check my-argocd’s DIFF. You can see replica is changing to 2!

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-58.png)

Click *SYN* C and *SYNCHRONIZE*.

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-56.png)

## Set my-argocd’s SYNC POLICY as Automatic

To set SYNC POLICY as Automatic, please change the manifest like bellow. Then, commit and push it. At the same time, let’s set replicas back to 1.

```
cat << EOF > my-argocd.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-argocd
spec:
  destination:
    name: ''
    namespace: argocd
    server: 'https://kubernetes.default.svc'
  source:
    path: ''
    repoURL: 'https://argoproj.github.io/argo-helm'
    targetRevision: 5.36.1
    chart: argo-cd
    helm:
      parameters:
        - name: applicationSet.livenessProbe.failureThreshold
          value: '4'
        - name: server.replicas
          value: '1'
    syncPolicy:
      automated: {}
  sources: []
  project: default
EOF
```
```
git add my-argocd.yaml \
&& git commit -m "Set syncPolicy=automated \
&& server.replicas=1" && git push
```

It is OK if you can confirm that number of argocd-server pod gets one!

![](https://yuki-nakamura.com/wp-content/uploads/2023/06/image-59.png)

## Wrap up

I explained how to implement GitOps with ArgoCD and GitHub. Additionally, we utilized the [App of Apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern) to manage the my-argocd Application through the argocd-resources Application. Lastly, we applied a GitOps approach to modify the parameters of the Helm Chart of ArgoCD and the Sync Policy of the Application.

In a total of three articles, including this one, I have explained the somewhat niche topic of managing ArgoCD with ArgoCD. From the next article, I would like to introduce how to practice GitOps for typical applications using ArgoCD!

Thank you for reading!