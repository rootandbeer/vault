---
categories:
  - "[[Clippings]]"
tags:
  - clippings
author:
  - "[[Md Sharjil Alam]]"
url: https://aws.plainenglish.io/kubernetes-explained-namespaces-pods-and-deployments-hands-on-627527f000c9
created: 2026-09-13
published: 2026-01-01
topics:
  - "[[Kubernetes]]"
---
**K8s Series — Part 2**

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*J17KCYUgDxhmUvXfYap6dw.png)

Kubernetes only truly clicks when theory meets practice. After exploring Kubernetes architecture in Part 1, it’s time to get your hands dirty.

In this article, you’ll learn:

- Why Namespaces exist and how to manage them
- How Pods run containers and share resources
- Why Deployments are essential in production
- How ReplicaSets fit into the picture
- Practical commands and tips you’ll see in interviews

This guide focuses on **doing, not just reading**.

📌 **Table of Contents**

1. Why Namespaces Exist
2. Creating Your First Namespace (YAML + Commands)
3. Pods: The Smallest Deployable Unit
4. Single vs Multi-Container Pods
5. Why Pods Are Not Enough
6. Deployments: Managing Pods Properly
7. Deployment → ReplicaSet → Pod
8. Deployment Strategies, Scaling, and Rollbacks
9. Key Interview Takeaways
10. What’s Next?

## 1️⃣ Why Namespaces Exist in Kubernetes

In real-world clusters:

- Multiple teams share a single cluster
- Multiple environments (dev, staging, prod) coexist
- Hundreds of resources run together

Namespaces provide **logical isolation**, making it easier to manage resources and avoid naming conflicts.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Pb2ZXX0xgyassH1t8fKzng.png)

Namespaces provide logical isolation of Kubernetes resources inside a single cluster.

> *📌 Important:  
> Namespaces* ***do not create separate clusters****. They only logically organize resources inside the same cluster.*

## 2️⃣ Creating a Namespace (YAML + Commands)

Kubernetes encourages **declarative configuration** using YAML instead of ad-hoc commands.

**namespace.yml**

```cs
apiVersion: v1
kind: Namespace
metadata:
  name: nginx
  labels:
    team: frontend
  annotations:
    purpose: demo
```

Apply it:

```cs
kubectl apply -f namespace.yml
kubectl get namespaces
```

> 📌 Interview Tip:  
> “Namespaces are cluster-scoped, but Pods, Deployments, and Services are namespace-scoped.  
> Default namespace is default. You can switch your kubectl context:”

```cs
kubectl config set-context --current --namespace=nginx
```

You can also set **resource quotas** to prevent one namespace from hogging cluster resources:

```cs
apiVersion: v1
kind: ResourceQuota
metadata:
  name: nginx-quota
  namespace: nginx
spec:
  hard:
    pods: "5"
    requests.cpu: "2"
    requests.memory: 4Gi
```

## 3️⃣ Pods: The Smallest Deployable Unit

A **Pod** is the smallest unit Kubernetes can deploy.

**Key facts:**

- Can run **one or more containers**
- Containers in the same Pod **share network (IP) and storage (volumes)**
- Pods are **ephemeral**: if a Pod dies, it doesn’t automatically restart unless managed by a higher-level controller

**Creating a Pod inside the nginx namespace**

**pod.yml**

```cs
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  namespace: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

Apply and verify:

```cs
kubectl apply -f pod.yml
kubectl get pods -n nginx
```

> *📌 Key Insight:  
> “* Directly managing Pods is fine for experiments, but **they lack self-healing, scaling, and rolling updates**.”

## 4️⃣ Single vs Multi-Container Pods

Pods can run multiple containers, which is useful for scenarios like the **sidecar pattern**.

## Get Md Sharjil Alam’s stories in your inbox

Join Medium for free to get updates from this writer.

**Single-container Pod**

```cs
apiVersion: v1
kind: Pod
metadata:
  name: single-container-pod
  namespace: nginx
spec:
  containers:
    - name: main-app
      image: main-app:latest
```

**Multi-container Pod**

```cs
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
  namespace: nginx
spec:
  containers:
    - name: main-app
      image: main-app:latest
    - name: sidecar-logger
      image: logger:latest
```

> *Containers in the same Pod* ***share network and storage****, allowing seamless communication.*

## 5️⃣ Why Pods Are Not Enough in Production

Directly using Pods creates challenges:

- **No self-healing:** A crashed Pod must be recreated manually
- **No scaling:** You can’t easily increase replicas
- **No rolling updates:** Manual replacement can cause downtime

> *✅ Golden Rule:* ***Deploy Deployments, not Pods***

## 6️⃣ Deployments: Managing Pods the Right Way

A **Deployment**:

- Manages Pods indirectly
- Ensures desired state is maintained
- Provides **scaling, rolling updates, and self-healing**

**deployment.yml**

```cs
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

Apply and verify:

```cs
kubectl apply -f deployment.yml
kubectl get deployments -n nginx
kubectl get rs -n nginx
kubectl get pods -n nginx
```

## 7️⃣ Deployment → ReplicaSet → Pod

Kubernetes uses this hierarchy:

```cs
Deployment
   ↓
ReplicaSet
   ↓
Pods
```
- Deployments **do not create Pods directly**
- ReplicaSets ensure the **desired number of Pods** are always running
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*3jcRYzQqIQ7hZ-SRhZ_uxQ.png)

> *📌 Interview Gold: Understanding this chain is essential.*

## 8️⃣ Deployment Strategies, Scaling, and Rollbacks

**Scaling a Deployment:**

```cs
kubectl scale deployment nginx-deployment --replicas=3
kubectl get pods -n nginx
```

**Rolling Updates:**

```cs
kubectl set image deployment/nginx-deployment nginx=nginx:1.21
kubectl rollout status deployment/nginx-deployment
```

**Rollback if something breaks:**

```cs
kubectl rollout undo deployment/nginx-deployment
```

**Pause & resume rollouts:**

```cs
kubectl rollout pause deployment/nginx-deployment
kubectl rollout resume deployment/nginx-deployment
```

**Deployment Strategies:**

- `RollingUpdate` (default): updates Pods gradually
- `Recreate`: deletes all old Pods first, then creates new ones

> *These strategies ensure* ***high availability*** *and zero downtime during updates.*

## 9️⃣ Key Takeaways (Interview-Ready)

- **Namespace**: logical isolation inside a cluster
- **Pods**: smallest deployable unit, ephemeral, and stateless
- **Containers in Pods**: can be single or multi-container (sidecar pattern)
- **Deployments**: manage Pods via ReplicaSets, enable scaling, self-healing, and rolling updates
- **Desired vs Actual State**: Kubernetes core principle
- Commands like `kubectl scale`, `rollout status`, `rollout undo` are practical interview points

🏷️ **Tags:**  
#Kubernetes #K8s #DevOps #CloudNative #Containers #LearningKubernetes