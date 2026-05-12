# Kubernetes Deployment Notes

> Beginner Friendly | Interview Focused | Hands-on Practical Guide

---

# Kubernetes Deployment Introduction

A **Deployment** in Kubernetes is used to manage Pods and ReplicaSets.

Deployments provide:

* Scaling
* Rolling updates
* Rollbacks
* Self-healing
* High availability

---

# 1. How many Pods exist on the system?

## Question

How many Pods exist in the current(default) namespace?

---

## Command

```bash
kubectl get pods
```

OR

```bash
kubectl get pods --no-headers | wc -l
```

---

## Explanation

* `kubectl get pods` lists all Pods in the current namespace.
* `--no-headers` removes table headings.
* `wc -l` counts the number of Pods.

---

## Sample Output

```bash
NAME                        READY   STATUS    RESTARTS   AGE
nginx-6799fc88d8-5r4wq     1/1     Running   0          5m
redis-6f7d8c9c6f-7plx2     1/1     Running   0          3m
```

Count Output:

```bash
2
```

---

## Important Notes

| Concept           | Explanation                            |
| ----------------- | -------------------------------------- |
| Pod               | Smallest deployable unit in Kubernetes |
| Namespace         | Logical isolation inside cluster       |
| Default Namespace | Used when namespace is not specified   |

---

## Common Errors

### Error

```bash
No resources found in default namespace
```

### Reason

No Pods exist in the namespace.

---

---

# 2. How many ReplicaSets exist on the system?

## Question

How many ReplicaSets exist in the current(default) namespace?

---

## Command

```bash
kubectl get replicaset
```

OR

```bash
kubectl get rs
```

---

## Explanation

ReplicaSet maintains the desired number of Pods.

Deployments automatically create ReplicaSets.

---

## Sample Output

```bash
NAME                  DESIRED   CURRENT   READY   AGE
nginx-6799fc88d8      3         3         3       10m
```

---

## Important Notes

| Resource   | Purpose                     |
| ---------- | --------------------------- |
| ReplicaSet | Maintains desired Pod count |
| Deployment | Manages ReplicaSets         |

---

## Common Errors

### Error

```bash
No resources found
```

### Reason

No ReplicaSets created yet.

---

---

# 3. How many Deployments exist on the system?

## Question

How many Deployments exist in the current(default) namespace?

---

## Command

```bash
kubectl get deployments
```

OR

```bash
kubectl get deploy
```

---

## Explanation

Deployment manages:

* ReplicaSets
* Pod updates
* Scaling
* Rollbacks

---

## Sample Output

```bash
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
nginx     3/3     3            3           15m
```

---

## Important Notes

| Feature         | Benefit                   |
| --------------- | ------------------------- |
| Rolling Updates | Zero downtime deployments |
| Rollback        | Restore previous version  |
| Scaling         | Increase/decrease Pods    |

---

## Common Errors

### Error

```bash
No resources found
```

### Reason

No Deployments exist.

---

---

# 4. How many Deployments exist on the system now?

## Question

We just created a Deployment! Check again!

---

## Command

```bash
kubectl get deployments
```

---

## Explanation

Checks Deployments after creating a new Deployment resource.

---

## Sample Output

```bash
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
nginx        3/3     3            3           15m
frontend     2/2     2            2           2m
```

---

## Important Notes

Deployment automatically creates:

1. ReplicaSet
2. Pods

Architecture Flow:

```text
Deployment
    ↓
ReplicaSet
    ↓
Pods
```

---

---

# 5. What is the image used to create the Pods in the new Deployment?

## Question

Find the container image used in the Deployment.

---

## Command

```bash
kubectl describe deployment <deployment-name>
```

Example:

```bash
kubectl describe deployment nginx
```

---

## Alternative Command

```bash
kubectl get deployment <deployment-name> -o yaml
```

---

## Explanation

The Deployment definition contains the container image information.

---

## Sample Output

```bash
Containers:
  nginx:
    Image: nginx:1.25
```

---

## Important Notes

| Field | Description     |
| ----- | --------------- |
| image | Container image |
| tag   | Image version   |

---

## Common Errors

### Error

```bash
Error from server (NotFound)
```

### Reason

Incorrect Deployment name.

---

---

# 6. Why do you think the Deployment is not ready?

## Question

Deployment shows NOT READY status.

---

## Commands

```bash
kubectl get deployments
```

```bash
kubectl describe deployment <deployment-name>
```

```bash
kubectl get pods
```

---

## Explanation

Deployment may not be ready because:

* Pods are crashing
* Image pull failures
* Readiness probe failures
* Resource shortage
* Invalid image name

---

## Common Real-World Issues

| Problem          | Description           |
| ---------------- | --------------------- |
| ImagePullBackOff | Incorrect image       |
| CrashLoopBackOff | Application crash     |
| Pending          | No node resources     |
| ErrImagePull     | Registry access issue |

---

## Sample Output

```bash
Warning  Failed     Failed to pull image
```

---

## Important Notes

Use Pod description for troubleshooting:

```bash
kubectl describe pod <pod-name>
```

---

---

# 7. Create Deployment using deployment-definition-1.yaml

## Question

Create a Deployment using the YAML file.

---

## Command

```bash
kubectl apply -f deployment-definition-1.yaml
```

---

## Issue in YAML

The YAML contains indentation problems.

---

## Correct YAML

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

spec:
  replicas: 1

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
        image: nginx

        command:
        - sh
        - "-c"
        - echo Hello Kubernetes! && sleep 3600
```

---

## Explanation

This Deployment:

* Creates 1 Pod
* Uses nginx image
* Executes custom shell command
* Keeps container running

---

## Important Notes

| Field    | Purpose                   |
| -------- | ------------------------- |
| replicas | Number of Pods            |
| selector | Matches Pod labels        |
| template | Pod configuration         |
| command  | Overrides startup command |

---

## Common Errors

### Error

```bash
error converting YAML to JSON
```

### Reason

Improper indentation.

---

## YAML Validation Command

```bash
kubectl apply --dry-run=client -f deployment-definition-1.yaml
```

---

---

# 8. Understanding Container Command Section

## YAML Snippet

```yaml
command:
- sh
- "-c"
- echo Hello Kubernetes! && sleep 3600
```

---

## Explanation

Overrides the default container startup command.

---

## Breakdown

| Part                   | Meaning              |
| ---------------------- | -------------------- |
| sh                     | Shell interpreter    |
| -c                     | Execute command      |
| echo Hello Kubernetes! | Print message        |
| sleep 3600             | Keep container alive |

---

## Real-World Use Case

Useful for:

* Debugging containers
* Temporary Pods
* Startup testing

---

---

# 9. kubectl explain deployment | head -n3

## Command

```bash
kubectl explain deployment | head -n3
```

---

## Explanation

Displays Kubernetes Deployment documentation directly from CLI.

---

## Sample Output

```bash
KIND:     Deployment
VERSION:  apps/v1
```

---

## Useful Commands

```bash
kubectl explain deployment.spec
```

```bash
kubectl explain deployment.spec.template
```

```bash
kubectl explain deployment.spec.replicas
```

---

## Interview Tip

`kubectl explain` is very useful during:

* Interviews
* YAML writing
* Troubleshooting

---

---

# 10. Create Deployment - httpd-frontend

## Question

Create a Deployment with:

* Name: `httpd-frontend`
* Replicas: `3`
* Image: `httpd:2.4-alpine`

---

## Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: httpd-frontend

spec:
  replicas: 3

  selector:
    matchLabels:
      name: httpd-frontend

  template:
    metadata:
      labels:
        name: httpd-frontend

    spec:
      containers:
      - name: httpd-frontend
        image: httpd:2.4-alpine
```

---

## Apply YAML

```bash
kubectl apply -f httpd-deployment.yaml
```

---

## Verify Deployment

```bash
kubectl get deployments
```

```bash
kubectl get pods
```

---

## Sample Output

```bash
NAME              READY   UP-TO-DATE   AVAILABLE
httpd-frontend    3/3     3            3
```

---

## Important Notes

| Component  | Purpose             |
| ---------- | ------------------- |
| Deployment | Manages application |
| ReplicaSet | Maintains Pod count |
| Pod        | Runs containers     |

---

## Common Errors

### Error

```bash
selector does not match template labels
```

### Reason

Labels mismatch between selector and template.

---

---

# 11. Difference Between kubectl create and kubectl apply

## kubectl create

### Command

```bash
kubectl create -f deployment.yaml
```

---

### Explanation

Creates resource only once.

If resource already exists:

```bash
Error from server (AlreadyExists)
```

---

# kubectl apply

## Command

```bash
kubectl apply -f deployment.yaml
```

---

## Explanation

* Creates resource if not present
* Updates resource if already exists
* Preferred in DevOps and CI/CD

---

# Comparison Table

| Feature                   | kubectl create | kubectl apply |
| ------------------------- | -------------- | ------------- |
| Creates Resource          | Yes            | Yes           |
| Updates Existing Resource | No             | Yes           |
| Declarative               | No             | Yes           |
| CI/CD Friendly            | Limited        | Excellent     |
| Re-runnable               | No             | Yes           |

---

# Real-World Recommendation

| Use Case               | Recommended Command |
| ---------------------- | ------------------- |
| First-time creation    | create              |
| Production deployments | apply               |
| CI/CD pipelines        | apply               |
| GitOps workflows       | apply               |

---

# Interview Answer

## Short Version

* `create` is imperative
* `apply` is declarative

`apply` is preferred in real-world Kubernetes environments.

---

# Useful Kubernetes Deployment Commands

## Deployment Commands

```bash
kubectl get deployments
kubectl describe deployment <name>
kubectl delete deployment <name>
kubectl scale deployment <name> --replicas=5
kubectl rollout status deployment <name>
kubectl rollout history deployment <name>
kubectl rollout undo deployment <name>
```

---

## Pod Commands

```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- sh
```

---

## YAML Validation

```bash
kubectl apply --dry-run=client -f file.yaml
```

---

## Output Formats

```bash
kubectl get deployment -o yaml
kubectl get deployment -o wide
kubectl get deployment -o json
```

---

# Kubernetes Deployment Architecture

```text
Deployment
    ↓
ReplicaSet
    ↓
Pods
    ↓
Containers
```

---

# Important Interview Concepts

| Concept        | Description                    |
| -------------- | ------------------------------ |
| Deployment     | Manages stateless applications |
| ReplicaSet     | Maintains desired Pod count    |
| Rolling Update | Updates Pods gradually         |
| Rollback       | Revert failed deployment       |
| Scaling        | Increase/decrease replicas     |
| Self-Healing   | Recreates failed Pods          |

---

# Final Summary

Deployments are one of the most important Kubernetes resources because they provide:

* Automated Pod management
* Scaling
* Rollbacks
* Rolling updates
* High availability
* Self-healing

Deployments are heavily used in real-world DevOps projects for managing production applications inside Kubernetes clusters.
