---
categories:
  - "[[Clippings]]"
tags:
  - "clippings"
author:
  - "[[Gary Turovsky]]"
url: "https://spin.atomicobject.com/k3s-deploy-docker-app-linux/"
created: 2026-09-06
published: 2026-04-16
topics:
---
Running and deploying with Kubernetes may sound excessive and intimidating, but it’s not nearly as hard as it once was. The immense popularity of Kubernetes over the past decade means tools can quickly set up and deploy a cluster in almost any environment. And what once was a technology platform requiring deep systems knowledge to use and understand is now straightforward to set up. This is doubly true if you have the help of an AI friend.

Other deployment approaches seem simpler on the surface. I’ve seen a bunch. You could automate the process of packaging up your container, transferring it to the Linux machine, and starting it up (say, as a systemd service or with Docker Compose). Alternatively, you could also check out your repo on the machine itself and build and launch the app there. You might know of some other approaches.

These other deployment methods can be effective, but they have drawbacks. Typically, the first drawback is either facing a lot of manual steps or writing a lot of tooling yourself. Another is the problem of friction — where every operating challenge (keeping the service running, scaling, dev/prod environments, logging, monitoring, etc.) is something that you have to solve yourself. You’re also using tooling that is “non-standard”, so transferring the knowledge of how to use it becomes harder. And, of course, should your app be so successful that it requires more Linux machines, deploying and managing more machines becomes a headache.

There’s a way that scales better. Here’s the stack I’d like to introduce you to, starting with k3s. There are several alternatives here, and you may face a situation where these don’t quite fit. But, I think once you see the possibilities, you’ll easily find a way to adapt or swap these out for something more appropriate.

## The Stack

- **[K3s](https://k3s.io/)** — This distribution of Kubernetes is designed to run on IoT and edge devices. It’s easy to deploy because it’s just a single binary, it has a great installer, and it’s fully compatible with the entire Kubernetes ecosystem of tooling. You can deploy it in an “air-gapped” environment. It comes with everything you need, including a CNI (Flannel), ingress controller (Traefik), a registry mirror, local path persistent volume provisioner, and more. And, if you’re not familiar with any of those, you won’t need to configure most of it.
- **[Helm](https://helm.sh/)** — Technically, you can just write YAML files directly to deploy to Kubernetes, but it’s not fun. Helm is great because it lets us draw on a large pool of pre-built “charts”. This works like a package manager for your favorite language (npm, pip, cargo, etc.), except it usually accepts some configuration via a values.yaml file you provide. It also lets you create a Chart to deploy your own containers.
- **[Zot](https://zotregistry.dev/)** — We can run this container registry in our Kubernetes cluster. Often, you’ll have a Linux server in an environment without a place to push your containers. This is a great solution because it’s easy to deploy, you can configure it to retain only a few recent images (to save disk space), and it has a fairly user-friendly web UI. Its security features are also pretty good.
- **[CloudNativePG](https://cloudnative-pg.io/)** — This is a PostgreSQL “operator”. We’ll define an operator as something you can add to your Kubernetes cluster to make provisioning of some resource more convenient. Here, the resource will be Postgres, which we’ll deploy as a single instance. This operator can also handle provisioning high-availability Postgres clusters (which may or may not make sense on a single Linux instance). With a little more effort than we cover in this article, you can also configure monitoring and automated backups.

## The Scenario

Okay, so let’s imagine we’re in a situation where we’ve written a fairly simple web app. We can build it into a single Docker container and it just requires an external Postgres database to run. We want the app deployed into an internal network, we’ve set up a Linux server, we have a VPN connection to the internal network, and SSH access to the box. Your actual scenario may be very different, but a lot of this deployment process can be adapted to meet your needs.

## The Process

We’ll set up everything from scratch: the cluster, a private container registry, a managed PostgreSQL instance, and a Helm-deployed app.

By the end, you’ll have:

- A single-node k3s cluster
- A private Zot OCI registry for your images
- A CloudNativePG-managed PostgreSQL instance
- A Helm chart deploying your app with automatic database wiring

## Prerequisites

- A Linux machine (Ubuntu 22.04+ or similar) with at least 2GB RAM
- A local development machine with Docker installed
- SSH access to the Linux box
- Your application’s Docker image (we’ll use a Node.js app as the example)

---

## Part 1: The Cluster

### Install k3s

> **On the server** — SSH in and run:

```bash
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="\
  --write-kubeconfig-mode 0644 \
  --tls-san 10.0.0.50" sh -
```

Replace `10.0.0.50` with your server’s actual IP address. This IP appears throughout the guide — use the same value everywhere.

Two flags worth noting:

- `--write-kubeconfig-mode 0644` makes the kubeconfig readable by non-root users. Without this, only root can run kubectl.
- `--tls-san` adds your server’s IP to the API server’s TLS certificate. Without it, kubectl from your laptop will fail with a certificate error because the cert is only valid for `127.0.0.1`.

Verify it’s running:

```bash
kubectl get nodes
```

```bash
NAME       STATUS   ROLES                  AGE   VERSION
myserver   Ready    control-plane,master   30s   v1.34.5+k3s1
```

### Configure Local Access

> **On your local machine** — copy the kubeconfig:

```bash
scp user@10.0.0.50:/etc/rancher/k3s/k3s.yaml ~/.kube/config-k3s
```

The file references `127.0.0.1` as the API server address. Replace it with your server’s real IP:

```bash
sed -i '' 's/127.0.0.1/10.0.0.50/g' ~/.kube/config-k3s
```

Set it as your active kubeconfig:

```bash
export KUBECONFIG=~/.kube/config-k3s
kubectl get nodes
```

### Install Helm

> **On your local machine** — if you don’t already have Helm:

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Helm uses the same `KUBECONFIG` environment variable, so it will automatically talk to your k3s cluster.

---

## Part 2: The Container Registry

We’ll deploy Zot inside the cluster and expose it on a NodePort.

### Install Zot via Helm

> **On your local machine:**

```bash
helm repo add project-zot https://zotregistry.dev/helm-charts
helm repo update
```

Create a values file (zot-values.yaml):

```yaml
replicaCount: 1
image:
  repository: ghcr.io/project-zot/zot
  tag: "v2.1.15"
  
service:
  type: NodePort
  port: 5000
  nodePort: 30500

persistence: true
pvc:
  create: true
  accessModes: ["ReadWriteOnce"]
  storage: 10Gi

mountConfig: true

configFiles:
  config.json: |-
    {
      "storage": {
        "rootDirectory": "/var/lib/registry"
      },
      "http": {
        "address": "0.0.0.0",
        "port": "5000"
      },
      "log": {
        "level": "info"
      }
    }
```

Deploy it:

```bash
helm install zot project-zot/zot \
  --namespace registry --create-namespace \
  -f zot-values.yaml
```

Verify it’s healthy:

```
curl http://10.0.0.50:30500/v2/
```

You should get an HTTP 200 response.

### Configure k3s to Pull from Zot

k3s uses containerd, which defaults to HTTPS for registry connections. Since Zot is running plain HTTP, you need to configure an explicit mirror.

> **On the server** — create /etc/rancher/k3s/registries.yaml:

```yaml
mirrors:
  "10.0.0.50:30500":
    endpoint:
      - "http://10.0.0.50:30500"
```

Restart k3s to pick up the change:

```bash
sudo systemctl restart k3s
```

### Configure Your Local Docker to Push

> **On your local machine** — tell Docker this registry doesn’t use TLS. Edit ~/.docker/daemon.json (or use Docker Desktop’s settings UI):

```json
{
  "insecure-registries": ["10.0.0.50:30500"]
}
```

Restart Docker, then test the push workflow:

```bash
docker build -t myapp:latest .
docker tag myapp:latest 10.0.0.50:30500/myapp:latest
docker push 10.0.0.50:30500/myapp:latest
```

---

## Part 3: The Database

Now we install the CNPG operator so it’s ready to create databases when our Helm chart asks for one.

### Install the CloudNativePG Operator

> **On your local machine:**

```bash
helm repo add cnpg https://cloudnative-pg.github.io/charts
helm repo update
helm install cnpg cnpg/cloudnative-pg \
  --namespace cnpg-system --create-namespace
```

This installs only the operator — no databases yet. Those are created by the CNPG Cluster resource in our app’s Helm chart.

---

## Part 4: The Application

With the cluster, registry, and database operator in place, the Helm chart ties them together. It deploys four things:

1. A CNPG PostgreSQL Cluster (your database)
2. A Deployment (your app container)
3. A Service (internal networking)
4. An Ingress (routes port 80 traffic to your app via Traefik)

Everything in Part 4 is on your local machine.

### Scaffold the chart

```bash
helm create myapp
cd myapp
rm -rf templates/tests templates/hpa.yaml templates/serviceaccount.yaml \
  templates/NOTES.txt templates/ingress.yaml templates/httproute.yaml
```

Delete the boilerplate you won’t need (we’ll replace deployment.yaml, service.yaml, and values.yaml shortly).

### The PostgreSQL Cluster (templates/pg-cluster.yaml)

This tells CNPG to create a single-instance PostgreSQL 17 cluster:

```yaml
apiVersion: postgresql.cnpg.io/v1

kind: Cluster

metadata:
  name: {{ .Release.Name }}-pg
  namespace: {{ .Release.Namespace }}

spec:
  instances: {{ .Values.database.instances }}
  imageName: {{ .Values.database.imageName }}
  bootstrap:
    initdb:
      database: {{ .Values.database.name }}
      owner: {{ .Values.database.owner }}
  storage:
    size: {{ .Values.database.storage }}
```

When applied, CNPG creates a PostgreSQL pod and initializes the database. It also generates a secret called `<release>-pg-app` — this is important because the Deployment template references it to get the database connection string.

The secret contains keys like `uri`, `host`, `port`, `username`, `password`, and `dbname`. The `uri` key is a ready-to-use connection string:

```
postgresql://myuser:s3cret@myapp-pg-rw.default.svc:5432/mydb
```

### The Deployment (templates/deployment.yaml)

Your app container needs two things from the cluster: the DATABASE\_URL (from the CNPG secret) and readiness confirmation that Postgres is accepting connections.

```yaml
apiVersion: apps/v1

kind: Deployment

metadata:
  name: {{ .Release.Name }}
  labels:
    app: {{ .Release.Name }}

spec:
  replicas: {{ .Values.app.replicas }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}

  template:
    metadata:
      labels:
        app: {{ .Release.Name }}

    spec:
      # Wait for PostgreSQL before starting the app
      initContainers:
        - name: wait-for-postgres
          image: postgres:17-alpine
          command:
            - sh
            - -c
            - |

              until pg_isready -h {{ .Release.Name }}-pg-rw -p 5432; do
                echo "Waiting for PostgreSQL..."
                sleep 2
              done

      containers:

        - name: app
          image: "{{ .Values.image.registry }}/{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: Always
          ports:
            - containerPort: {{ .Values.app.port }}

          env:
            # Wired directly from the CNPG-generated secret
            - name: DATABASE_URL
              valueFrom:
                secretKeyRef:
                  name: {{ .Release.Name }}-pg-app
                  key: uri
          livenessProbe:
            httpGet:
              path: /api/health
              port: {{ .Values.app.port }}
            initialDelaySeconds: 30
            periodSeconds: 15

          readinessProbe:
            httpGet:
              path: /api/health
              port: {{ .Values.app.port }}
            initialDelaySeconds: 15
            periodSeconds: 10
```

Key details:

- **The init container** uses postgres:17-alpine solely for its `pg_isready` command — it waits until PostgreSQL is accepting connections, then exits. Without this, your app will crash-loop while Postgres is still starting up.
- **{{.Release.Name }}-pg-app** under **secretKeyRef** is the auto-generated secret. You never create this — CNPG does it when the Cluster resource is reconciled.
- **imagePullPolicy: Always** — without it, Kubernetes caches the image and misses your updates.

### The Service (templates/service.yaml)

The Service gives your Deployment a stable internal address:

```yaml
apiVersion: v1

kind: Service

metadata:
  name: {{ .Release.Name }}

spec:
  type: ClusterIP
  ports:
    - port: {{ .Values.app.port }}
      targetPort: {{ .Values.app.port }}

  selector:
    app: {{ .Release.Name }}
```

### The Ingress (templates/ingress.yaml)

k3s ships with Traefik listening on port 80. This Ingress routes all incoming traffic to your app:

```yaml
apiVersion: networking.k8s.io/v1

kind: Ingress

metadata:
  name: {{ .Release.Name }}

spec:
  ingressClassName: traefik
  rules:
    - http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: {{ .Release.Name }}
                port:
                  number: {{ .Values.app.port }}
```

### Values (values.yaml)

```yaml
image:
  registry: "10.0.0.50:30500"
  repository: myapp
  tag: latest

app:
  port: 3000
  replicas: 1

database:
  instances: 1
  imageName: "ghcr.io/cloudnative-pg/postgresql:17"
  name: myapp
  owner: myapp_user
  storage: 5Gi
```

---

## Part 5: Deploy

> **On your local machine** — build and push your image using the workflow from Part 2, then deploy:

```bash
helm install myapp ./myapp --namespace myapp --create-namespace
```

Watch the pods come up:

```bash
kubectl get pods -n myapp -w
```

You’ll see the CNPG PostgreSQL pod start first, then the init container in your app pod waiting for it, and finally your app container starting once Postgres is ready:

```bash
NAME                     READY   STATUS     RESTARTS   AGE
myapp-pg-1               1/1     Running    0          45s
myapp-6d4f8b7c9f-x2k7p  0/1     Init:0/1   0          45s
myapp-6d4f8b7c9f-x2k7p  0/1     PodInitializing   0   52s
myapp-6d4f8b7c9f-x2k7p  1/1     Running    0          60s
```

Verify the app is healthy:

```
curl http://10.0.0.50/api/health
```

---

## Wrapping Up

Your app is now running on port 80 of a single Linux server, backed by a managed PostgreSQL instance, with images stored in a private registry — all on a Kubernetes cluster you set up in minutes.

Here’s what’s running:

| Component | How to Access |
| --- | --- |
| **k3s** | kubectl via kubeconfig |
| **Zot** | docker push 10.0.0.50:30500/… |
| **CNPG + PostgreSQL** | Internal only (myapp-pg-rw:5432) |
| **Your App** | http://10.0.0.50 |

Total footprint: all of the above runs comfortably on 2-4GB of RAM.

Since you’re on Kubernetes now, you can create a second deployment in a different [namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) to run production and development environments side by side on the same machine. You should also consider configuring [backups](https://cloudnative-pg.io/docs/1.28/backup) for your Postgres instances.