# Kubernetes Pods Basics – Commands & Explanations

This document contains:
- Kubernetes Pods basic questions
- Commands
- Explanations
- YAML examples
- Important concepts

Perfect for:
- Interview Preparation
- Kubernetes Revision
- GitHub Notes Repository
- Beginners Learning Kubernetes

---

# Question 1

## How many pods exist on the system?
In the current(default) namespace.

### Command
```bash
kubectl get pods
```

### Explanation
- `kubectl get pods` lists all pods in the current namespace.
- By default Kubernetes uses the `default` namespace.
- Count the number of rows under the `NAME` column.

### Sample Output
```bash
NAME         READY   STATUS    RESTARTS   AGE
nginx        1/1     Running   0          2m
webapp       1/1     Running   0          1m
```

---

# Question 2

## Create a new pod using the nginx image.

### Command
```bash
kubectl run nginx --image=nginx
```

### Explanation
- `kubectl run` creates a pod.
- `nginx` → pod name.
- `--image=nginx` → image to use for the container.

### Verify Pod Creation
```bash
kubectl get pods
```

---

# Question 3

## How many pods are created now?

### Command
```bash
kubectl get pods
```

### Explanation
- Re-run the command after creating the new pod.
- Compare the total number of pods before and after creation.

---

# Question 4

## Which image is specified for the pods whose names begin with the newpods- prefix?

### Command
```bash
kubectl describe pod <pod-name>
```

### Example
```bash
kubectl describe pod newpods-fef6f
```

### Explanation
- `kubectl describe pod` shows detailed pod information.
- Look for the `Image:` field.

### Example Output
```bash
Containers:
  nginx-container:
    Image: nginx
```

---

# Question 5

## Which nodes are these pods placed on?

### Command
```bash
kubectl get pods -o wide
```

### Explanation
- `-o wide` provides additional details.
- Check the `NODE` column.

### Example Output
```bash
NAME         READY   STATUS    NODE
nginx        1/1     Running   node01
webapp       1/1     Running   node02
```

---

# Question 6

## We just created a new pod named webapp. How many containers are part of the webapp pod?

### Command
```bash
kubectl describe pod webapp
```

### Explanation
- Under the `Containers:` section count the containers listed.
- A pod may contain:
  - Single container
  - Multiple containers

### Example
```bash
Containers:
  nginx-container
  agentx
```

This means:
- Total containers = 2

---

# Question 7

## What images are used in the new webapp pod?

### Command
```bash
kubectl describe pod webapp
```

### Explanation
- Look for all `Image:` fields under the container section.

### Example Output
```bash
Containers:
  nginx-container:
    Image: nginx

  agentx:
    Image: agentx:v2
```

---

# Question 8

## What is the state of the container agentx in the pod webapp?

### Command
```bash
kubectl describe pod webapp
```

### Explanation
- Find the container named `agentx`.
- Look for the `State:` field.

### Example Output
```bash
State: Waiting
Reason: CrashLoopBackOff
```

### Common Container States
| State | Meaning |
|---|---|
| Running | Container is running |
| Waiting | Container is starting |
| Terminated | Container stopped |
| CrashLoopBackOff | Container repeatedly crashing |

---

# Question 9

## Why do you think the container agentx in pod webapp is in error?

### Command
```bash
kubectl describe pod webapp
```

### Explanation
- Scroll to the `Events:` section.
- Kubernetes events explain why the container failed.

### Example Output
```bash
Failed to pull image "agentx:v2"
```

### Common Reasons
- Wrong image name
- ImagePullBackOff
- CrashLoopBackOff
- Port conflict
- Resource issues
- Missing environment variables

---

# Question 10

## What does the READY column in the output of the kubectl get pods command indicate?

### Command
```bash
kubectl get pods
```

### Explanation
The `READY` column format is:

```text
<ready-containers>/<total-containers>
```

### Example 1
```text
1/1
```

Meaning:
- 1 container ready out of 1 total container.

### Example 2
```text
1/2
```

Meaning:
- 1 container ready out of 2 containers.

---

# Question 11

## Delete the webapp Pod.

### Command
```bash
kubectl delete pod webapp
```

### Explanation
- Deletes the pod from the cluster.
- Kubernetes gracefully terminates the pod.

### Verify Deletion
```bash
kubectl get pods
```

---

# Question 12

## Create a new pod with the name redis and the image redis123.

### YAML File
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis

spec:
  containers:
    - name: redis
      image: redis123
```

### Save File As
```text
redis-definition.yaml
```

### Apply YAML File
```bash
kubectl apply -f redis-definition.yaml
```

### Explanation
| Field | Meaning |
|---|---|
| apiVersion | Kubernetes API version |
| kind | Resource type |
| metadata | Pod details |
| spec | Pod specification |
| containers | Container configuration |
| image | Container image |

### Important
- `redis123` is intentionally incorrect.
- Do NOT correct it initially.

---

# Question 13

## Now change the image on this pod to redis.

### Command
```bash
kubectl edit pod redis
```

### Update This Line
```yaml
image: redis123
```

### Change To
```yaml
image: redis
```

### Save and Exit

For Vi Editor:
```bash
:wq
```

### Explanation
- `kubectl edit` opens the live pod configuration.
- Update the image name.
- Kubernetes recreates the container using the correct image.

---

# Alternative Method

## Use kubectl set image

### Command
```bash
kubectl set image pod/redis redis=redis
```

### Explanation
Syntax:
```bash
kubectl set image <resource-type>/<resource-name> <container-name>=<image>
```

---

# Question 14

## Use the kubectl edit command to update the image of the pod to redis.

### Command
```bash
kubectl edit pod redis
```

### Update Section
```yaml
image: redis
```

### Save and Exit
```bash
:wq
```

### Verify Pod Status
```bash
kubectl get pods
```

### Expected Output
```bash
NAME      READY   STATUS    RESTARTS   AGE
redis     1/1     Running   0          20s
```

---

# Important Kubernetes Pod Commands

---

## List Pods
```bash
kubectl get pods
```

---

## List Pods with More Details
```bash
kubectl get pods -o wide
```

---

## Describe Pod
```bash
kubectl describe pod <pod-name>
```

Example:
```bash
kubectl describe pod nginx
```

---

## View Pod Logs
```bash
kubectl logs <pod-name>
```

Example:
```bash
kubectl logs nginx
```

---

## Watch Pod Status Continuously
```bash
kubectl get pods -w
```

---

## Delete Pod
```bash
kubectl delete pod <pod-name>
```

Example:
```bash
kubectl delete pod nginx
```

---

## Get Pod YAML
```bash
kubectl get pod <pod-name> -o yaml
```

Example:
```bash
kubectl get pod nginx -o yaml
```

---

## Create Pod Using YAML
```bash
kubectl apply -f pod-definition.yaml
```

---

## Edit Existing Pod
```bash
kubectl edit pod <pod-name>
```

---

## Set New Image
```bash
kubectl set image pod/<pod-name> <container-name>=<image>
```

Example:
```bash
kubectl set image pod/redis redis=redis
```

---

# Important Kubernetes Concepts

---

## What is a Pod?
- Smallest deployable object in Kubernetes.
- Runs one or more containers.
- Containers inside a pod share:
  - Network
  - Storage
  - Namespace

---

## What is kubectl?
- Kubernetes command line tool.
- Used to:
  - Create resources
  - Manage clusters
  - Troubleshoot issues

---

## What is YAML?
- Human-readable configuration format.
- Used to define Kubernetes objects.

---

## What is ContainerCreating?
- Kubernetes is preparing the container.
- Pulling image.
- Creating networking.
- Mounting volumes.

---

## What is CrashLoopBackOff?
- Container continuously crashes.
- Kubernetes keeps restarting it.

---

## What is ImagePullBackOff?
- Kubernetes unable to pull container image.
- Usually due to:
  - Wrong image name
  - Private registry access issue
  - Internet/network issue

---

# Summary

This document covers:
- Pod creation
- Pod deletion
- Pod inspection
- YAML basics
- Container states
- Image troubleshooting
- kubectl commands

Best for:
- Kubernetes beginners
- Interview preparation
- Daily DevOps revision
- GitHub documentation
- Hands-on practice

---
