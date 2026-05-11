# Kubernetes ReplicaSet Notes

## What is a ReplicaSet?

A ReplicaSet ensures that a specified number of identical PODs are always running in the Kubernetes cluster.

### Real-World Example

If your application should always have:
- 3 nginx PODs running
- Even if one POD crashes

ReplicaSet automatically creates a new POD to maintain the desired count.

---

# Kubernetes ReplicaSet Architecture

```text
ReplicaSet
    |
    |---- Pod 1
    |---- Pod 2
    |---- Pod 3
```

ReplicaSet continuously monitors POD health and count.

---

# 1. How many PODs exist on the system?

## Question

How many PODs exist on the system in the current namespace?

---

## Command

```bash
kubectl get pods
```

---

## Explanation

This command lists all PODs in the current namespace.

By default:
- Kubernetes uses the `default` namespace
- Only PODs from that namespace are shown

---

## Sample Output

```bash
NAME                     READY   STATUS    RESTARTS   AGE
new-replica-set-abcde   1/1     Running   0          5m
new-replica-set-fghij   1/1     Running   0          5m
new-replica-set-klmno   1/1     Running   0          5m
new-replica-set-pqrst   1/1     Running   0          5m
```

---

## Important Notes

| Field | Meaning |
|---|---|
| READY | Containers ready inside POD |
| STATUS | Current POD state |
| RESTARTS | Number of restarts |
| AGE | POD age |

---

# 2. How many ReplicaSets exist on the system?

## Question

How many ReplicaSets exist in the current namespace?

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

This command lists all ReplicaSets in the current namespace.

---

## Sample Output

```bash
NAME              DESIRED   CURRENT   READY   AGE
new-replica-set   4         4         0       5m
```

---

## Important Notes

| Column | Meaning |
|---|---|
| DESIRED | Number of PODs expected |
| CURRENT | Current running PODs |
| READY | Ready PODs |
| AGE | ReplicaSet age |

---

# 3. How many PODs are DESIRED in the new-replica-set?

## Question

How many PODs are desired in `new-replica-set`?

---

## Command

```bash
kubectl get rs
```

---

## Explanation

The `DESIRED` column indicates how many PODs ReplicaSet wants to maintain.

---

## Sample Output

```bash
NAME              DESIRED   CURRENT   READY
new-replica-set   4         4         0
```

---

## Answer

Desired POD count = **4**

---

# 4. What image is used to create the PODs in new-replica-set?

## Question

Which container image is used in `new-replica-set`?

---

## Command

```bash
kubectl describe rs new-replica-set
```

---

## Explanation

`kubectl describe` provides detailed information about ReplicaSet including:
- Labels
- Selectors
- POD template
- Image used

---

## Sample Output

```bash
Containers:
  busybox:
    Image: busybox777
```

---

## Answer

Image used = `busybox777`

---

## Important Notes

ReplicaSet creates PODs using:
- Container image
- Labels
- Template defined in YAML

---

# 5. Why are the PODs not ready?

## Question

Why are PODs in `new-replica-set` not becoming READY?

---

## Command

```bash
kubectl get pods
```

OR

```bash
kubectl describe pod <pod-name>
```

---

## Explanation

PODs may not become ready because:
- Wrong image name
- Image pull failure
- CrashLoopBackOff
- Container startup issues

In this scenario:
- Image used = `busybox777`
- This image does not exist

---

## Sample Output

```bash
Failed to pull image "busybox777"
ErrImagePull
ImagePullBackOff
```

---

## Common Errors

| Error | Meaning |
|---|---|
| ErrImagePull | Kubernetes failed to download image |
| ImagePullBackOff | Kubernetes retrying image pull |

---

## Real-World Understanding

If Docker image does not exist:
- Kubernetes cannot create containers
- POD stays in failed state

---

# 6. Delete one POD directly

## Question

Delete one POD manually.

---

## Command

```bash
kubectl delete pod <pod-name>
```

Example:

```bash
kubectl delete pod new-replica-set-abcde
```

---

## Explanation

ReplicaSet continuously maintains desired count.

If one POD is deleted:
- ReplicaSet automatically creates a new POD

---

## Sample Output

```bash
pod "new-replica-set-abcde" deleted
```

---

## Verify

```bash
kubectl get pods
```

You will notice:
- Old POD deleted
- New POD automatically created

---

## Important Kubernetes Concept

### Self-Healing

ReplicaSet provides:
- Automatic recovery
- Automatic POD recreation
- High availability

---

# 7. Create ReplicaSet using replicaset-definition-1.yaml

## Question

Fix the issue and create ReplicaSet.

---

## Incorrect YAML

```yaml
apiVersion: v1
kind: ReplicaSet
metadata:
  name: replicaset-1
```

---

## Problem

ReplicaSet belongs to:
- `apps/v1`
- NOT `v1`

---

## Correct YAML

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-1
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## Command

```bash
kubectl create -f replicaset-definition-1.yaml
```

---

## Sample Output

```bash
replicaset.apps/replicaset-1 created
```

---

## Important Notes

| Field | Purpose |
|---|---|
| selector | Identifies matching PODs |
| template | POD template |
| replicas | Desired POD count |

---

## Kubernetes Concept

ReplicaSet requires:
- Matching labels
- Matching selectors

Otherwise ReplicaSet will fail.

---

# 8. Fix replicaset-definition-2.yaml

## Question

Fix the issue and create ReplicaSet.

---

## Incorrect YAML

```yaml
selector:
  matchLabels:
    tier: frontend

template:
  metadata:
    labels:
      tier: nginx
```

---

## Problem

Selector labels and POD labels must match.

---

## Correct YAML

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-2
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## Command

```bash
kubectl create -f replicaset-definition-2.yaml
```

---

## Important Notes

### Selector Matching Rule

These must match:

```yaml
selector:
  matchLabels:
```

AND

```yaml
template:
  metadata:
    labels:
```

---

## Common Error

```bash
selector does not match template labels
```

---

# 9. Delete ReplicaSets

## Question

Delete:
- replicaset-1
- replicaset-2

---

## Command

```bash
kubectl delete rs replicaset-1 replicaset-2
```

---

## Explanation

Deleting ReplicaSet also deletes:
- Managed PODs

Unless orphaning is used.

---

## Sample Output

```bash
replicaset.apps "replicaset-1" deleted
replicaset.apps "replicaset-2" deleted
```

---

# 10. Fix new-replica-set image

## Question

Update image from invalid image to:
- `busybox`

---

## Option 1 — Edit Existing ReplicaSet

### Command

```bash
kubectl edit rs new-replica-set
```

---

## Update Image

Replace:

```yaml
image: busybox777
```

With:

```yaml
image: busybox
```

---

## Delete Existing PODs

```bash
kubectl delete pods --all
```

---

## Explanation

ReplicaSet automatically creates new PODs using updated image.

---

## Verify

```bash
kubectl get pods
```

---

## Option 2 — Delete and Recreate

### Delete ReplicaSet

```bash
kubectl delete rs new-replica-set
```

---

### Recreate

```bash
kubectl create -f new-replica-set.yaml
```

---

## Important Notes

ReplicaSet does NOT automatically update existing PODs after template modification.

You must:
- Delete PODs manually
OR
- Recreate ReplicaSet

---

# 11. Scale ReplicaSet to 5 PODs

## Question

Scale ReplicaSet to 5 PODs.

---

## Command

```bash
kubectl scale rs new-replica-set --replicas=5
```

---

## Explanation

ReplicaSet immediately creates additional PODs.

---

## Verify

```bash
kubectl get rs
```

---

## Sample Output

```bash
NAME              DESIRED   CURRENT   READY
new-replica-set   5         5         5
```

---

## Real-World Use Case

Scaling helps during:
- High traffic
- Increased workload
- Peak business hours

---

# 12. Scale ReplicaSet down to 2 PODs

## Question

Scale ReplicaSet to 2 PODs.

---

## Command

```bash
kubectl scale rs new-replica-set --replicas=2
```

---

## Explanation

ReplicaSet removes extra PODs automatically.

---

## Verify

```bash
kubectl get pods
```

---

## Sample Output

```bash
NAME                     READY   STATUS
new-replica-set-abcd     1/1     Running
new-replica-set-efgh     1/1     Running
```

---

## Real-World Use Case

Scaling down helps:
- Reduce cloud cost
- Save CPU and memory
- Optimize infrastructure

---

# Important Kubernetes ReplicaSet Concepts

| Concept | Explanation |
|---|---|
| ReplicaSet | Maintains desired POD count |
| Self-Healing | Automatically recreates failed PODs |
| Scaling | Increase/decrease PODs |
| Labels | Used to identify objects |
| Selectors | Match labels |
| Template | Blueprint for POD creation |

---

# Important kubectl Commands for ReplicaSet

| Command | Purpose |
|---|---|
| kubectl get rs | List ReplicaSets |
| kubectl describe rs <name> | Detailed ReplicaSet info |
| kubectl create -f file.yaml | Create ReplicaSet |
| kubectl delete rs <name> | Delete ReplicaSet |
| kubectl edit rs <name> | Edit ReplicaSet |
| kubectl scale rs <name> --replicas=5 | Scale ReplicaSet |
| kubectl get pods | List PODs |
| kubectl describe pod <name> | Detailed POD info |

---

# Interview Questions on ReplicaSet

## Difference Between ReplicationController and ReplicaSet

| ReplicationController | ReplicaSet |
|---|---|
| Older technology | Newer technology |
| Equality-based selector only | Set-based selectors supported |
| Deprecated | Recommended |

---

## Difference Between ReplicaSet and Deployment

| ReplicaSet | Deployment |
|---|---|
| Maintains POD count | Manages ReplicaSets |
| No rolling updates | Supports rolling updates |
| Basic scaling | Advanced deployment management |

---

# Best Practices

- Always use `apps/v1`
- Ensure labels and selectors match
- Use meaningful labels
- Use Deployments instead of ReplicaSets in production
- Verify images before deployment

---

# Final Summary

ReplicaSet is responsible for:
- Maintaining POD count
- Self-healing
- Scaling applications
- Ensuring high availability

In production:
- ReplicaSet is usually managed by Deployments
- Rarely created directly

---
