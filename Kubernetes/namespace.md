# Kubernetes Namespace Notes

> Beginner Friendly | Interview Focused | Practical Kubernetes Namespace Guide

---

# Kubernetes Namespace Introduction

A **Namespace** in Kubernetes is used to logically divide cluster resources.

Namespaces help in:

* Environment separation
* Resource isolation
* Team management
* Access control
* Organizing applications

---

# Real-World Example

| Namespace  | Purpose                   |
| ---------- | ------------------------- |
| dev        | Development environment   |
| test       | Testing environment       |
| prod       | Production environment    |
| monitoring | Monitoring tools          |
| finance    | Finance team applications |

---

# Default Kubernetes Namespaces

| Namespace       | Purpose                              |
| --------------- | ------------------------------------ |
| default         | Default namespace for user workloads |
| kube-system     | Kubernetes internal components       |
| kube-public     | Publicly accessible resources        |
| kube-node-lease | Node heartbeat information           |

---

---

# 1. How many Namespaces exist on the system?

## Question

How many Namespaces exist in the Kubernetes cluster?

---

## Command

```bash id="3i2o6m"
kubectl get namespaces
```

OR

```bash id="yd8sv2"
kubectl get ns
```

---

## Explanation

This command lists all Namespaces available in the cluster.

---

## Sample Output

```bash id="7t0od6"
NAME              STATUS   AGE
default           Active   5d
kube-system       Active   5d
kube-public       Active   5d
research          Active   2d
finance           Active   1d
marketing         Active   1d
```

---

## Count Namespaces

```bash id="dhukwz"
kubectl get ns --no-headers | wc -l
```

---

## Important Notes

| Concept       | Description                       |
| ------------- | --------------------------------- |
| Namespace     | Logical separation inside cluster |
| Isolation     | Prevents resource conflicts       |
| Multi-Tenancy | Supports multiple teams           |

---

## Common Errors

### Error

```bash id="ktk2h6"
No resources found
```

### Reason

Usually happens in restricted environments or permission issues.

---

---

# 2. How many Pods exist in the research namespace?

## Question

Find the number of Pods running inside the `research` namespace.

---

## Command

```bash id="pnrfw0"
kubectl get pods -n research
```

---

## Count Pods

```bash id="tgh5v0"
kubectl get pods -n research --no-headers | wc -l
```

---

## Explanation

* `-n` stands for namespace
* Lists Pods only from the specified namespace

---

## Sample Output

```bash id="5yr66i"
NAME           READY   STATUS    AGE
redis-pod      1/1     Running   2h
nginx-pod      1/1     Running   1h
```

Count:

```bash id="1h0brf"
2
```

---

## Important Notes

| Command          | Purpose                   |
| ---------------- | ------------------------- |
| kubectl get pods | List Pods                 |
| -n research      | Target research namespace |

---

## Common Errors

### Error

```bash id="8vth75"
Error from server (NotFound): namespaces "research" not found
```

### Reason

Namespace does not exist.

---

---

# 3. Create a POD in the finance namespace

## Question

Create a Pod in the `finance` namespace.

### Requirements

| Attribute | Value |
| --------- | ----- |
| Name      | redis |
| Image     | redis |

---

# Imperative Command Method

## Command

```bash id="xxe2g5"
kubectl run redis --image=redis -n finance
```

---

## Explanation

This command:

* Creates a Pod named `redis`
* Uses Redis image
* Deploys Pod inside `finance` namespace

---

## Verify Pod

```bash id="v9xy3j"
kubectl get pods -n finance
```

---

## Sample Output

```bash id="vz69sk"
NAME    READY   STATUS    RESTARTS   AGE
redis   1/1     Running   0          30s
```

---

# Declarative YAML Method

## YAML File

```yaml id="g5i96n"
apiVersion: v1
kind: Pod

metadata:
  name: redis
  namespace: finance

spec:
  containers:
  - name: redis
    image: redis
```

---

## Apply YAML

```bash id="7b8yb5"
kubectl apply -f redis-pod.yaml
```

---

## Important Notes

| Method      | Description                 |
| ----------- | --------------------------- |
| Imperative  | Quick command-line creation |
| Declarative | YAML-based configuration    |

---

## Common Errors

### Error

```bash id="y5gm5o"
Error from server (NotFound): namespaces "finance" not found
```

### Reason

Namespace must exist before creating Pod.

---

## Create Namespace First

```bash id="q2j3pb"
kubectl create namespace finance
```

---

---

# 4. Which namespace has the blue Pod in it?

## Question

Find the Namespace where the `blue` Pod exists.

---

## Command

```bash id="rw2d7m"
kubectl get pods --all-namespaces
```

OR

```bash id="7g81p7"
kubectl get pods -A
```

---

## Explanation

* `-A` means all namespaces
* Lists Pods across the cluster

---

## Sample Output

```bash id="tm5ysc"
NAMESPACE    NAME       READY   STATUS
marketing    blue       1/1     Running
```

---

## Important Notes

| Option           | Meaning        |
| ---------------- | -------------- |
| -A               | All namespaces |
| --all-namespaces | Same as -A     |

---

## Common Errors

### Error

```bash id="j2l4nl"
No resources found
```

### Reason

Pod may not exist.

---

---

# 5. Shortest DNS name for redis-db-service in same namespace

## Question

What is the shortest DNS name the Blue application should use to access `redis-db-service` in the same namespace (`marketing`)?

Port: `6379`

---

## Answer

```text id="fw1zt9"
redis-db-service
```

---

## Explanation

When applications are in the same namespace:

* Kubernetes DNS automatically resolves service names
* Only service name is required

---

## Real-World Concept

Inside same namespace:

```text id="e0x4by"
Application → Service Name → Target Service
```

---

## Important Notes

| Scenario            | DNS Name             |
| ------------------- | -------------------- |
| Same Namespace      | redis-db-service     |
| Different Namespace | redis-db-service.dev |

---

---

# 6. DNS name for redis-db-service in dev namespace

## Question

What DNS name should the Blue application use to access `redis-db-service` in the `dev` namespace?

Port: `6379`

---

## Short DNS Format

```text id="9ol1g4"
redis-db-service.dev
```

---

## Fully Qualified Domain Name (FQDN)

```text id="wf6jyl"
redis-db-service.dev.svc.cluster.local
```

---

## Explanation

Since applications are in different namespaces:

* Namespace name must be included
* Kubernetes DNS resolves service location

---

## DNS Structure Breakdown

| Part             | Meaning        |
| ---------------- | -------------- |
| redis-db-service | Service name   |
| dev              | Namespace      |
| svc              | Service type   |
| cluster.local    | Cluster domain |

---

## Real-World Example

```text id="s88brw"
frontend.dev → backend.prod
```

Requires namespace reference for communication.

---

## Important Notes

| Access Type         | DNS Format                          |
| ------------------- | ----------------------------------- |
| Same Namespace      | service-name                        |
| Different Namespace | service-name.namespace              |
| Full DNS            | service.namespace.svc.cluster.local |

---

---

# 7. Imperative Commands in Kubernetes

# What are Imperative Commands?

## Simple Definition

> Imperative = "Do this now."

You directly tell Kubernetes what action to perform immediately using CLI commands.

---

# Common Imperative Operations

| Operation | Description                |
| --------- | -------------------------- |
| Create    | Create resources           |
| Delete    | Remove resources           |
| Scale     | Increase/decrease replicas |
| Expose    | Create Services            |
| Run       | Create Pods                |

---

# Example

## Imperative Command

```bash id="7rl8ef"
kubectl create deployment nginx --image=nginx
```

---

## Meaning

“Kubernetes, create a Deployment named nginx using nginx image.”

---

# Explanation

This command immediately:

1. Creates Deployment
2. Creates ReplicaSet
3. Creates Pods

without writing YAML files.

---

# Verify Deployment

```bash id="8kg3rr"
kubectl get deployments
```

---

# Sample Output

```bash id="kmt1yw"
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           20s
```

---

# Common Imperative Commands

## Create Namespace

```bash id="jlwm4v"
kubectl create namespace dev
```

---

## Run Pod

```bash id="s8ybxw"
kubectl run nginx --image=nginx
```

---

## Create Deployment

```bash id="v4d4rt"
kubectl create deployment nginx --image=nginx
```

---

## Scale Deployment

```bash id="k1kxzw"
kubectl scale deployment nginx --replicas=3
```

---

## Expose Deployment

```bash id="cfc71u"
kubectl expose deployment nginx --port=80 --type=NodePort
```

---

## Delete Pod

```bash id="j1sjfu"
kubectl delete pod nginx
```

---

# Imperative vs Declarative

| Feature        | Imperative   | Declarative |
| -------------- | ------------ | ----------- |
| Method         | CLI Commands | YAML Files  |
| Speed          | Fast         | Slower      |
| Reusable       | No           | Yes         |
| Best For       | Quick tasks  | Production  |
| CI/CD Friendly | Limited      | Excellent   |

---

# Real-World Recommendation

| Use Case              | Recommended |
| --------------------- | ----------- |
| Learning Kubernetes   | Imperative  |
| Quick Testing         | Imperative  |
| Production Deployment | Declarative |
| GitOps/CI-CD          | Declarative |

---

# Important Namespace Commands

## Get Namespaces

```bash id="d3z8wl"
kubectl get ns
```

---

## Create Namespace

```bash id="3dhm9n"
kubectl create namespace dev
```

---

## Delete Namespace

```bash id="mjlwm8"
kubectl delete namespace dev
```

---

## Run Pod in Namespace

```bash id="gk63rn"
kubectl run nginx --image=nginx -n dev
```

---

## Get Pods in Namespace

```bash id="ynpw9d"
kubectl get pods -n dev
```

---

## Switch Namespace Temporarily

```bash id="zv5i6f"
kubectl config set-context --current --namespace=dev
```

---

# Kubernetes DNS Concepts

| DNS Type            | Example                             |
| ------------------- | ----------------------------------- |
| Same Namespace      | redis-service                       |
| Different Namespace | redis-service.dev                   |
| Full DNS            | redis-service.dev.svc.cluster.local |

---

# Kubernetes Namespace Architecture

```text id="w6n9we"
Cluster
 ├── default
 ├── kube-system
 ├── dev
 ├── finance
 └── marketing
```

---

# Interview Questions

| Question                       | Short Answer                    |
| ------------------------------ | ------------------------------- |
| What is Namespace?             | Logical isolation in Kubernetes |
| Why use Namespace?             | Resource separation             |
| Default Namespace?             | default                         |
| Command to list namespaces?    | kubectl get ns                  |
| Same namespace service access? | service-name                    |
| Different namespace access?    | service-name.namespace          |

---

# Final Summary

Namespaces are one of the most important Kubernetes concepts for:

* Resource isolation
* Multi-team environments
* Environment separation
* Access control
* Kubernetes DNS communication

In real-world DevOps projects, Namespaces are heavily used to separate:

* Development
* Testing
* Production
* Monitoring
* Team-specific workloads
