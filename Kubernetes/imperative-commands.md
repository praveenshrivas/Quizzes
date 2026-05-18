# Kubernetes Imperative Commands Notes

> Beginner-Friendly | Interview-Focused | Hands-On Kubernetes Notes  
> Topic: Kubernetes Imperative Commands

---

# Table of Contents

1. What are Imperative Commands?
2. Deploy an NGINX Pod
3. Deploy Redis Pod with Labels
4. Expose Redis Pod using ClusterIP Service
5. Create Deployment with Replicas
6. Create Pod with Custom Container Port
7. Create Namespace
8. Create Deployment in Namespace
9. Create Pod + Service Quickly
10. List API Resources
11. Shortname for HorizontalPodAutoscaler
12. Namespaced vs Cluster Scoped Resources
13. Describe Pod Resource
14. Explore YAML Structure using kubectl explain
15. Explore Pod Spec Containers Type
16. Deployment Replicas Field Type
17. kubectl explain --recursive
18. Explore service.spec.ports Structure
19. Important kubectl Commands
20. Common Interview Questions

---

# 1. What are Imperative Commands?

Imperative commands directly tell Kubernetes what action to perform immediately using the CLI.

Instead of creating YAML files, we execute commands directly using `kubectl`.

---

## Imperative Example

```bash
kubectl run nginx --image=nginx
```

---

## Declarative Example

```bash
kubectl apply -f pod.yaml
```

---

## When to Use Imperative Commands

| Use Case | Why |
|---|---|
| Learning Kubernetes | Fast and simple |
| Interviews | Frequently asked |
| CKA/CKAD Exams | Saves time |
| Testing | Quick deployments |
| Debugging | Easy troubleshooting |

---

## Important Note

Imperative commands are useful for:

- Labs
- Practice
- Interviews
- Quick testing

Production environments usually prefer:

- YAML files
- GitOps
- Helm
- Terraform

---

# 2. Deploy a Pod Named nginx-pod using nginx:alpine

## Question

Deploy a pod named `nginx-pod` using the `nginx:alpine` image.

Use imperative commands only.

---

## Command

```bash
kubectl run nginx-pod --image=nginx:alpine
```

---

## Explanation

| Part | Meaning |
|---|---|
| kubectl run | Creates a pod |
| nginx-pod | Pod name |
| --image | Container image |

---

## Sample Output

```bash
pod/nginx-pod created
```

---

## Verify

```bash
kubectl get pods
```

---

## Important Notes

- `kubectl run` creates a Pod directly.
- Pods are created in the `default` namespace unless specified.

---

## Common Errors

### Error

```bash
ImagePullBackOff
```

### Cause

- Incorrect image name
- Registry issue
- Internet connectivity issue

---

# 3. Deploy Redis Pod using Labels

## Question

Deploy a Redis pod using the `redis:alpine` image with label:

```text
tier=db
```

---

## Command

```bash
kubectl run redis-pod \
--image=redis:alpine \
--labels="tier=db" \
--port=6379
```

---

## Explanation

| Flag | Purpose |
|---|---|
| --labels | Adds labels |
| --port | Defines container port |

---

## Real-World Concept

Labels are used for:

- Service selection
- Monitoring
- Grouping resources
- Environment tagging

Example:

```text
app=frontend
tier=backend
env=prod
```

---

## Verify Labels

```bash
kubectl get pods --show-labels
```

---

## Sample Output

```bash
NAME        READY   STATUS    LABELS
redis-pod   1/1     Running   tier=db
```

---

## Important Notes

Services use labels to identify pods.

---

# 4. Create ClusterIP Service for Redis Pod

## Question

Create a service named `redis-service` to expose the existing Redis pod internally on port `6379`.

Type: `ClusterIP`

---

## Command

```bash
kubectl expose pod redis-pod \
--port=6379 \
--name=redis-service \
--type=ClusterIP
```

---

## Explanation

| Part | Meaning |
|---|---|
| kubectl expose | Creates service |
| pod redis-pod | Target pod |
| --port | Service port |
| --type=ClusterIP | Internal service |

---

## Kubernetes Concept

### ClusterIP

- Default service type
- Accessible only inside cluster
- Used for internal communication

---

## Sample Output

```bash
service/redis-service exposed
```

---

## Verify

```bash
kubectl get svc
```

---

## Common Errors

### Error

```bash
error: couldn't find resource
```

### Cause

Wrong pod name used.

---

# 5. Create Deployment with 3 Replicas

## Question

Create deployment:

| Field | Value |
|---|---|
| Name | webapp |
| Image | kodekloud/webapp-color |
| Replicas | 3 |

---

## Command

```bash
kubectl create deployment webapp \
--image=kodekloud/webapp-color \
--replicas=3
```

---

## Explanation

| Part | Meaning |
|---|---|
| create deployment | Creates deployment |
| --replicas | Number of pod copies |

---

## Kubernetes Concept

### Deployment

Deployment manages:

- ReplicaSets
- Scaling
- Rolling updates
- Self-healing

---

## Verify

```bash
kubectl get deploy
```

```bash
kubectl get pods
```

---

## Sample Output

```bash
deployment.apps/webapp created
```

---

## Common Errors

### Error

```bash
unknown flag: --replicas
```

### Cause

Older kubectl versions may require scaling separately.

---

## Alternative

```bash
kubectl create deployment webapp --image=kodekloud/webapp-color
kubectl scale deployment webapp --replicas=3
```

---

# 6. Create Pod with Custom Container Port

## Question

Create pod:

| Field | Value |
|---|---|
| Name | custom-nginx |
| Image | nginx |
| Port | 8080 |

---

## Command

```bash
kubectl run custom-nginx \
--image=nginx \
--port=8080
```

---

## Explanation

`--port` defines container port metadata.

---

## Verify

```bash
kubectl describe pod custom-nginx
```

---

## Important Note

This does NOT expose the pod externally.

You still need a Service.

---

# 7. Create Namespace

## Question

Create namespace:

```text
dev-ns
```

---

## Command

```bash
kubectl create namespace dev-ns
```

---

## Explanation

Namespaces logically separate Kubernetes resources.

---

## Verify

```bash
kubectl get ns
```

---

## Real-World Usage

| Namespace | Purpose |
|---|---|
| dev | Development |
| qa | Testing |
| prod | Production |

---

# 8. Create Deployment inside Namespace

## Question

Create deployment:

| Field | Value |
|---|---|
| Name | redis-deploy |
| Namespace | dev-ns |
| Image | redis |
| Replicas | 2 |

---

## Command

```bash
kubectl create deployment redis-deploy \
--image=redis \
--replicas=2 \
-n dev-ns
```

---

## Verify

```bash
kubectl get deploy -n dev-ns
```

---

## Important Note

`-n` specifies namespace.

---

# 9. Create Pod + Service Quickly

## Question

Create:

- Pod named `httpd`
- Image `httpd:alpine`
- ClusterIP Service named `httpd`
- Port `80`

---

## Commands

### Step 1

```bash
kubectl run httpd \
--image=httpd:alpine \
--port=80
```

### Step 2

```bash
kubectl expose pod httpd \
--port=80 \
--type=ClusterIP \
--name=httpd
```

---

## Verify

```bash
kubectl get pods,svc
```

---

# 10. List All Available API Resources

## Question

Which command lists all Kubernetes API resources?

---

## Command

```bash
kubectl api-resources
```

---

## Sample Output

```bash
NAME         SHORTNAMES   APIVERSION
pods         po           v1
services     svc          v1
deployments  deploy       apps/v1
```

---

## Important Notes

Useful for:

- Finding resource names
- Discovering shortnames
- Understanding API groups

---

# 11. Shortname for HorizontalPodAutoscaler

## Question

What is the shortname for horizontalpodautoscalers?

---

## Command

```bash
kubectl api-resources | grep horizontalpodautoscalers
```

---

## Answer

```text
hpa
```

---

## Example

```bash
kubectl get hpa
```

---

# 12. Which Resource is NOT Namespaced?

## Question

Which resource is cluster-scoped?

---

## Examples of Cluster Scoped Resources

| Resource | Namespaced? |
|---|---|
| Node | No |
| Namespace | No |
| PersistentVolume | No |

---

## Examples of Namespaced Resources

| Resource | Namespaced? |
|---|---|
| Pod | Yes |
| Service | Yes |
| Deployment | Yes |

---

## Important Concept

Cluster-scoped resources belong to the entire cluster.

---

# 13. Describe Pod Resource

## Question

Which command describes Pod resource details?

---

## Command

```bash
kubectl describe pod nginx-pod
```

---

## Information Displayed

- Labels
- Events
- Container status
- Image
- IP
- Mounts

---

## Useful for Troubleshooting

Very important interview command.

---

# 14. Explore YAML Structure using kubectl explain

## Question

How to explore available Pod fields?

---

## Command

```bash
kubectl explain pod.spec.containers
```

---

## Explanation

Displays documentation for YAML fields.

---

## Real-World Usage

Useful while writing:

- Pod YAML
- Deployment YAML
- Service YAML

---

# 15. TYPE of containers Field

## Question

What is the TYPE of `containers` field?

---

## Command

```bash
kubectl explain pod.spec.containers
```

---

## Answer

```text
TYPE: []Container
```

---

## Meaning

- `[]` means array/list
- Multiple containers can exist in one pod

---

# 16. TYPE of replicas Field in Deployment

## Question

What is the TYPE of replicas field?

---

## Command

```bash
kubectl explain deployment.spec.replicas
```

---

## Answer

```text
TYPE: integer
```

---

## Explanation

Replicas define the number of pod copies.

---

# 17. What Does --recursive Do?

## Question

What does `--recursive` do?

---

## Command

```bash
kubectl explain pod --recursive
```

---

## Explanation

Displays complete nested structure recursively.

---

## Use Case

Helpful for:

- Learning YAML
- Writing manifests
- Interview preparation

---

# 18. Explore service.spec.ports Structure

## Question

Explore service.spec.ports recursively.

---

## Command

```bash
kubectl explain service.spec.ports --recursive
```

---

## Common Fields Available

| Field | Description |
|---|---|
| port | Service port |
| protocol | TCP/UDP |
| targetPort | Container port |
| nodePort | NodePort value |

---

## Important Note

`replicas` field is NOT available under `service.spec.ports`.

Because replicas belong to Deployments, not Services.

---

# 19. Important kubectl Commands

| Command | Purpose |
|---|---|
| kubectl get pods | List pods |
| kubectl get svc | List services |
| kubectl get deploy | List deployments |
| kubectl describe pod | Detailed pod info |
| kubectl logs pod-name | View logs |
| kubectl exec -it pod-name -- sh | Access container |
| kubectl delete pod pod-name | Delete pod |
| kubectl scale deployment app --replicas=3 | Scale deployment |
| kubectl rollout status deploy/app | Check rollout |
| kubectl api-resources | List resources |
| kubectl explain pod | YAML help |

---

# 20. Common Kubernetes Interview Questions

## What is the difference between Pod and Deployment?

| Pod | Deployment |
|---|---|
| Single running instance | Manages multiple pods |
| No self-healing | Self-healing |
| No scaling | Supports scaling |

---

## Why are labels important?

Labels help Kubernetes:

- Identify resources
- Select pods
- Route traffic
- Organize workloads

---

## What is ClusterIP?

ClusterIP is the default Kubernetes Service type used for internal communication inside the cluster.

---

## Difference Between Service Types

| Type | Access |
|---|---|
| ClusterIP | Internal |
| NodePort | External via Node IP |
| LoadBalancer | Cloud external LB |

---

# Final Notes

Imperative commands are extremely important for:

- CKA/CKAD exams
- Interviews
- Troubleshooting
- Quick deployments

But in real-world production environments:

- YAML
- Helm
- GitOps
- Terraform

are preferred for better maintainability.

---

# End of Notes
