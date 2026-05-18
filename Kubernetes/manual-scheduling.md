# Kubernetes Manual Scheduling

---

# Kubernetes Manual Scheduling - Beginner Friendly Notes

## Topic Overview

In Kubernetes, the Scheduler is responsible for assigning Pods to Nodes.

Normally:

```text
User -> Creates Pod -> Scheduler selects Node -> Pod runs
```

But in some scenarios:
- Scheduler may not exist
- Scheduler may be stopped
- Manual scheduling may be required

This lab demonstrates:
- Why a Pod stays in Pending state
- How Kubernetes scheduling works
- How to manually assign Pods to Nodes

---

# Lab Scenario

A Pod definition file named:

```bash
nginx.yaml
```

is already available.

Tasks:
1. Create the Pod
2. Inspect why it is Pending
3. Inspect control plane components
4. Manually schedule Pod on node01
5. Schedule Pod on controlplane node

---

# 1. Create the Pod

## Question

Create a Pod using the provided `nginx.yaml` file.

---

## Command

```bash
kubectl apply -f nginx.yaml
```

OR

```bash
kubectl create -f nginx.yaml
```

---

## Explanation

This command creates a Pod from the YAML definition file.

Kubernetes API Server receives the request and stores the object in etcd.

Normally:
- Scheduler detects unscheduled Pods
- Scheduler assigns a Node
- Kubelet starts the container

---

## Verify Pod Status

```bash
kubectl get pods
```

---

## Sample Output

```text
NAME    READY   STATUS    RESTARTS   AGE
nginx   0/1     Pending   0          5s
```

---

## Important Notes

| Status | Meaning |
|---|---|
| Pending | Pod created but not assigned/running |
| Running | Pod successfully running |
| CrashLoopBackOff | Container crashing repeatedly |

---

## Real-World Concept

In production:
- Pods first enter Pending state
- Scheduler decides the best node based on:
  - CPU
  - Memory
  - Affinity
  - Taints/Tolerations
  - Resource requests

---

---

# 2. Why is the Pod in Pending State?

## Question

Why is the Pod still in `Pending` state?

---

## Command

```bash
kubectl describe pod nginx
```

---

## Explanation

The `describe` command gives detailed Pod information including:
- Events
- Scheduling information
- Errors
- Container details

---

## Sample Output

```text
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  10s   default-scheduler  no nodes available
```

OR

```text
Pod has no node assigned
```

---

## Root Cause

The Pod is Pending because:
- Scheduler is not running
- OR Pod was not scheduled to any node

---

## Kubernetes Concept

### What Does Scheduler Do?

The Kubernetes Scheduler:
- Watches for unscheduled Pods
- Selects best Node
- Assigns Pod to Node

Without scheduler:
- Pods remain Pending forever

---

## Important Notes

| Component | Responsibility |
|---|---|
| API Server | Accepts requests |
| Scheduler | Assigns Pods to Nodes |
| Kubelet | Runs containers |
| etcd | Stores cluster data |

---

---

# 3. Inspect Kubernetes Control Plane Components

## Question

Inspect Kubernetes control plane components.

---

## Command

```bash
kubectl get pods --namespace kube-system
```

---

## Explanation

The `kube-system` namespace contains Kubernetes internal components.

This command helps identify:
- Scheduler status
- API Server
- Controller Manager
- DNS
- etc.

---

## Sample Output

```text
NAME                                   READY   STATUS
coredns-64897985d                      1/1     Running
etcd-controlplane                      1/1     Running
kube-apiserver-controlplane            1/1     Running
kube-controller-manager-controlplane   1/1     Running
```

Notice:
- Scheduler Pod may be missing

---

## Important Observation

If you do NOT see:

```text
kube-scheduler-controlplane
```

then scheduler is not running.

That is why Pod remains Pending.

---

## Real-World Explanation

Think of Scheduler as:
- Delivery manager assigning orders to delivery boys

Without manager:
- Orders exist
- But nobody assigns them

---

---

# 4. Manually Schedule the Pod on node01

## Question

Manually schedule the Pod on `node01`.

Delete and recreate the Pod if necessary.

---

# Step 1 - Delete Existing Pod

## Command

```bash
kubectl delete pod nginx
```

---

# Step 2 - Modify YAML

Add:

```yaml
nodeName: node01
```

inside Pod spec.

---

## Updated YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  nodeName: node01
  containers:
  - name: nginx
    image: nginx
```

---

## Explanation

Normally:
- Scheduler assigns node

Here:
- We manually assign node using `nodeName`

This bypasses Scheduler completely.

---

# Step 3 - Recreate Pod

## Command

```bash
kubectl apply -f nginx.yaml
```

---

# Step 4 - Verify

## Command

```bash
kubectl get pods -o wide
```

---

## Sample Output

```text
NAME    READY   STATUS    NODE
nginx   1/1     Running   node01
```

---

## Important Notes

| Field | Purpose |
|---|---|
| nodeName | Directly assigns Pod to Node |
| nodeSelector | Selects nodes using labels |
| affinity | Advanced scheduling rules |

---

## Kubernetes Concept

### Manual Scheduling

When `nodeName` is specified:
- Scheduler is skipped
- Kubelet on target node starts Pod directly

---

## Common Errors

### Error

```text
node "node01" not found
```

### Cause
Incorrect node name.

---

## Verify Nodes

```bash
kubectl get nodes
```

---

---

# 5. Schedule Pod on controlplane Node

## Question

Now schedule the same Pod on the `controlplane` node.

Delete and recreate Pod if necessary.

---

# Step 1 - Delete Existing Pod

```bash
kubectl delete pod nginx
```

---

# Step 2 - Modify YAML

Update:

```yaml
nodeName: controlplane
```

---

## YAML

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  nodeName: controlplane
  containers:
  - name: nginx
    image: nginx
```

---

# Step 3 - Recreate Pod

```bash
kubectl apply -f nginx.yaml
```

---

# Step 4 - Verify

```bash
kubectl get pods -o wide
```

---

## Sample Output

```text
NAME    READY   STATUS    NODE
nginx   1/1     Running   controlplane
```

---

# Important Kubernetes Concept

## Why Pods Normally Don't Run on Control Plane?

Control plane nodes are usually tainted:

```text
node-role.kubernetes.io/control-plane:NoSchedule
```

This prevents normal workloads from running there.

But:
- `nodeName` bypasses scheduler decisions
- Pod gets directly assigned

---

# Common Errors

## Error

```text
0/1 nodes are available
```

### Cause
Scheduler issue or taints.

---

## Error

```text
pod pending forever
```

### Cause
Scheduler missing or node unavailable.

---

---

# Manual Scheduling vs Scheduler

| Feature | Scheduler | Manual Scheduling |
|---|---|---|
| Automatic | Yes | No |
| Uses Resources | Yes | No |
| Intelligent Placement | Yes | No |
| Production Recommended | Yes | Rarely |
| Uses nodeName | No | Yes |

---

# Important kubectl Commands

## Pod Commands

```bash
kubectl get pods
kubectl describe pod nginx
kubectl delete pod nginx
kubectl logs nginx
```

---

## Node Commands

```bash
kubectl get nodes
kubectl describe node node01
```

---

## YAML Commands

```bash
kubectl apply -f nginx.yaml
kubectl create -f nginx.yaml
kubectl delete -f nginx.yaml
```

---

## kube-system Inspection

```bash
kubectl get pods -n kube-system
kubectl get all -n kube-system
```

---

# Interview Questions

## Q1. Why does a Pod remain in Pending state?

### Answer
A Pod remains Pending when:
- Scheduler cannot assign node
- Insufficient resources
- Node unavailable
- Taints/Tolerations mismatch
- Scheduler missing

---

## Q2. What is nodeName in Kubernetes?

### Answer
`nodeName` directly assigns a Pod to a specific node and bypasses Scheduler.

---

## Q3. What happens if Scheduler is down?

### Answer
New Pods remain Pending because no component assigns them to Nodes.

---

## Q4. What is the role of kube-scheduler?

### Answer
The Scheduler watches for unscheduled Pods and selects the best Node based on resource availability and scheduling policies.

---

# Final Summary

In this lab you learned:
- How Pods are scheduled
- Why Pods remain Pending
- Role of kube-scheduler
- How to inspect Kubernetes control plane components
- How to manually assign Pods to Nodes using `nodeName`
- Difference between automatic and manual scheduling

---

# Real DevOps Takeaway

Manual scheduling is rarely used in production, but understanding it helps you deeply understand:
- Kubernetes architecture
- Scheduler behavior
- Pod lifecycle
- Node assignment
- Cluster troubleshooting

This is a very common Kubernetes interview topic.

---
