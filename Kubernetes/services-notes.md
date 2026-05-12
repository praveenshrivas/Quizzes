# Kubernetes Services - Complete Beginner to Interview Notes

---

# Table of Contents

1. What is a Kubernetes Service?
2. Why Services are Needed
3. Types of Kubernetes Services
4. Question 1: How many Services exist on the system?
5. Question 2: What is the type of the default kubernetes service?
6. Question 3: What is the targetPort configured on the kubernetes service?
7. Question 4: How many labels are configured on the kubernetes service?
8. Question 5: How many Endpoints are attached on the kubernetes service?
9. Question 6: How many Deployments exist on the system now?
10. Question 7: What is the image used to create the pods in the deployment?
11. Question 8: Can you currently access the Web App UI through the tab?
12. Question 9: Create a new service to access the web application
13. Understanding Service Networking
14. Service YAML Deep Dive
15. Common Kubernetes Service Errors
16. Troubleshooting Commands
17. Important Interview Questions
18. Quick Revision Notes
19. Important kubectl Commands

---

# What is a Kubernetes Service?

A Kubernetes Service is an abstraction layer that exposes applications running inside Pods.

Pods are temporary and their IP addresses can change anytime.

A Service provides:
- Stable IP
- Stable DNS
- Load balancing
- Communication between applications

---

# Why Services are Needed

Without Services:
- Pods get random IPs
- Applications become unreachable
- Communication becomes difficult

With Services:
- Stable networking
- Reliable communication
- Easy application exposure

---

# Real-World Example

Imagine:
- Pods = Employees
- Service = Reception Desk

Employees may change, but the reception number remains the same.

---

# Types of Kubernetes Services

| Service Type | Usage |
|---|---|
| ClusterIP | Internal communication |
| NodePort | External access through node |
| LoadBalancer | Cloud external load balancer |
| ExternalName | Maps service to external DNS |

---

# ClusterIP

Default service type.

Accessible only inside cluster.

Example:
- Backend API
- Database communication

---

# NodePort

Exposes application outside cluster.

Accessible using:

```text
<NodeIP>:<NodePort>
```

Example:

```text
192.168.1.10:30080
```

---

# LoadBalancer

Creates cloud load balancer.

Used in:
- AWS
- Azure
- GCP

---

# ExternalName

Maps Kubernetes service to external DNS.

Example:

```yaml
externalName: google.com
```

---

---

# Question 1: How many Services exist on the system?

## Command

```bash
kubectl get services
```

OR

```bash
kubectl get svc
```

---

## Explanation

This command lists all Services in the current namespace.

Default namespace is used unless specified.

---

## Sample Output

```bash
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   10m
```

---

## Answer

```text
1 Service
```

---

## Important Notes

- `svc` is short form for services
- Services provide stable networking
- Services route traffic to Pods

---

## Kubernetes Concept

Service uses:
- Labels
- Selectors
- Endpoints

to connect with Pods.

---

---

# Question 2: What is the type of the default kubernetes service?

## Command

```bash
kubectl get svc
```

---

## Sample Output

```bash
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   10m
```

---

## Answer

```text
ClusterIP
```

---

## Explanation

The TYPE column shows the Service type.

Here:
- `kubernetes` service type is ClusterIP

---

## Kubernetes Concept

ClusterIP:
- Internal communication only
- Default service type
- Not accessible externally

---

## Important Notes

Every Kubernetes cluster automatically creates:
- kubernetes service

It points to:
- Kubernetes API Server

---

---

# Question 3: What is the targetPort configured on the kubernetes service?

## Command

```bash
kubectl describe svc kubernetes
```

---

## Sample Output

```bash
Name:              kubernetes
Namespace:         default
Labels:            component=apiserver
                   provider=kubernetes
Type:              ClusterIP
IP:                10.43.0.1
Port:              https  443/TCP
TargetPort:        6443/TCP
Endpoints:         192.168.0.1:6443
```

---

## Answer

```text
6443
```

---

## Explanation

Traffic flow:

```text
Service Port → TargetPort → Pod
```

- Service receives traffic on Port 443
- Forwards to container port 6443

---

# Understanding Ports

| Field | Meaning |
|---|---|
| port | Service port |
| targetPort | Container port |
| nodePort | External access port |

---

## Real-World Example

Like:
- Reception desk receives calls on one number
- Forwards to employee extension number

---

## Common Errors

### Error: Connection Refused

Cause:
- Wrong targetPort

Fix:
- Verify container listening port

---

---

# Question 4: How many labels are configured on the kubernetes service?

## Command

```bash
kubectl describe svc kubernetes
```

---

## Sample Output

```bash
Labels:
  component=apiserver
  provider=kubernetes
```

---

## Answer

```text
2 Labels
```

---

## Explanation

Labels are key-value metadata attached to objects.

---

# Label Example

```yaml
labels:
  app: nginx
  env: prod
```

---

## Kubernetes Concept

Labels help:
- Group resources
- Select Pods
- Organize applications

---

## Important Notes

Services use selectors to match Pod labels.

If labels mismatch:
- No traffic routing happens

---

---

# Question 5: How many Endpoints are attached on the kubernetes service?

## Command

```bash
kubectl describe svc kubernetes
```

---

## Sample Output

```bash
Endpoints: 192.168.0.1:6443
```

---

## Answer

```text
1 Endpoint
```

---

## Explanation

Endpoints are:
- Actual Pod IPs
- Container ports

Service forwards traffic to endpoints.

---

# Kubernetes Concept

Flow:

```text
Service → Endpoint → Pod
```

---

## Important Notes

If no endpoints exist:
- Service exists
- Application fails

---

## Common Errors

### Error: No Endpoints Available

Cause:
- Wrong selector
- Pod labels mismatch

Fix:
- Verify labels and selectors

---

---

# Question 6: How many Deployments exist on the system now?

## Command

```bash
kubectl get deployments
```

OR

```bash
kubectl get deploy
```

---

## Sample Output

```bash
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
simple-webapp    1/1     1            1           5m
```

---

## Answer

```text
1 Deployment
```

---

# What is a Deployment?

Deployment manages:
- Pods
- ReplicaSets
- Scaling
- Rolling updates

---

## Important Notes

Deployments provide:
- Self healing
- Auto recreation
- Easy scaling

---

## Real-World Example

If a Pod crashes:
- Deployment automatically recreates it

---

---

# Question 7: What is the image used to create the pods in the deployment?

## Command

```bash
kubectl describe deployment simple-webapp
```

---

## Sample Output

```bash
Image: kodekloud/simple-webapp
```

---

## Answer

```text
kodekloud/simple-webapp
```

---

# What is a Container Image?

Container image contains:
- Application code
- Runtime
- Dependencies
- Libraries

---

## Common Registries

| Registry | Description |
|---|---|
| Docker Hub | Public registry |
| Amazon ECR | AWS registry |
| GCR | Google registry |
| ACR | Azure registry |

---

## Common Errors

### Error: ImagePullBackOff

Cause:
- Wrong image name
- Invalid tag
- Private image access issue

Fix:
- Verify image details

---

---

# Question 8: Can you currently access the Web App UI through the tab?

## Answer

```text
No
```

---

## Explanation

Currently:
- Application is running inside Pods
- No external Service exists

Therefore:
- Web UI is inaccessible

---

# Kubernetes Concept

Pods are internal by default.

To expose externally:
- NodePort
- LoadBalancer
- Ingress

are required.

---

---

# Question 9: Create a new service to access the web application

---

# Requirements

| Field | Value |
|---|---|
| Name | webapp-service |
| Type | NodePort |
| targetPort | 8080 |
| port | 8080 |
| nodePort | 30080 |
| selector | name=simple-webapp |

---

# Service YAML

## File: service-definition-1.yaml

```yaml
apiVersion: v1
kind: Service

metadata:
  name: webapp-service

spec:
  type: NodePort

  ports:
    - targetPort: 8080
      port: 8080
      nodePort: 30080

  selector:
    name: simple-webapp
```

---

# Apply the YAML

## Command

```bash
kubectl apply -f service-definition-1.yaml
```

---

# Verify Service

## Command

```bash
kubectl get svc
```

---

## Sample Output

```bash
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes         ClusterIP   10.43.0.1       <none>        443/TCP          10m
webapp-service     NodePort    10.43.120.15    <none>        8080:30080/TCP   1m
```

---

# Traffic Flow

```text
User
 ↓
NodeIP:30080
 ↓
Service Port:8080
 ↓
TargetPort:8080
 ↓
Pod
```

---

# Understanding NodePort

NodePort exposes application externally using:
- Worker Node IP
- Specific port

NodePort range:
```text
30000-32767
```

---

# Service YAML Deep Dive

---

# apiVersion

```yaml
apiVersion: v1
```

Defines Kubernetes API version.

---

# kind

```yaml
kind: Service
```

Defines object type.

---

# metadata

```yaml
metadata:
  name: webapp-service
```

Stores:
- Name
- Labels
- Namespace

---

# spec

Defines Service behavior.

---

# type

```yaml
type: NodePort
```

Defines Service type.

---

# ports

```yaml
ports:
  - port: 8080
```

Defines Service port.

---

# targetPort

```yaml
targetPort: 8080
```

Defines container port.

---

# nodePort

```yaml
nodePort: 30080
```

Defines external node port.

---

# selector

```yaml
selector:
  name: simple-webapp
```

Matches Pod labels.

---

# Common Kubernetes Service Errors

---

# Error: No Endpoints

## Cause

Selector mismatch.

---

## Fix

Verify:

```bash
kubectl get pods --show-labels
```

---

# Error: Connection Refused

## Cause

Wrong targetPort.

---

## Fix

Verify container port.

---

# Error: Service Not Accessible

## Possible Causes

| Cause | Fix |
|---|---|
| Pod not running | Check Pods |
| Wrong selector | Verify labels |
| Wrong port | Verify ports |
| Firewall issue | Open nodePort |

---

# Error: CrashLoopBackOff

## Cause

Application crashing repeatedly.

---

## Fix

Check logs:

```bash
kubectl logs <pod-name>
```

---

# Important Troubleshooting Commands

---

# Get Services

```bash
kubectl get svc
```

---

# Describe Service

```bash
kubectl describe svc webapp-service
```

---

# Get Endpoints

```bash
kubectl get endpoints
```

---

# Get Pods

```bash
kubectl get pods
```

---

# Show Labels

```bash
kubectl get pods --show-labels
```

---

# Describe Pod

```bash
kubectl describe pod <pod-name>
```

---

# Check Logs

```bash
kubectl logs <pod-name>
```

---

# Delete Service

```bash
kubectl delete svc webapp-service
```

---

# Edit Service

```bash
kubectl edit svc webapp-service
```

---

# Expose Deployment Quickly

```bash
kubectl expose deployment simple-webapp --type=NodePort --port=8080
```

---

# Port Forwarding

```bash
kubectl port-forward svc/webapp-service 8080:8080
```

---

# Service Selector Example

---

# Service

```yaml
selector:
  app: nginx
```

---

# Pod Labels

```yaml
labels:
  app: nginx
```

---

# Important Concept

If labels match:
- Traffic works

If labels do not match:
- No endpoints created

---

# Kubernetes Networking Flow

```text
User
 ↓
Service
 ↓
Endpoint
 ↓
Pod
 ↓
Container
```

---

# Important Interview Questions

| Question | Answer |
|---|---|
| Why Services are needed? | Stable networking |
| What is ClusterIP? | Internal Service |
| What is NodePort? | External access |
| Difference between port and targetPort? | Service port vs Container port |
| What happens if selector mismatches? | No endpoints |
| Default service type? | ClusterIP |
| NodePort range? | 30000-32767 |
| What is endpoint? | Pod IP + Port |
| What is LoadBalancer service? | Cloud LB integration |

---

# Quick Revision Notes

| Concept | Key Point |
|---|---|
| Service | Stable access |
| ClusterIP | Internal communication |
| NodePort | External communication |
| selector | Matches Pods |
| labels | Organize objects |
| endpoint | Actual Pod IP |
| targetPort | Container port |
| port | Service port |

---

# Complete Architecture Understanding

```text
Deployment
   ↓
ReplicaSet
   ↓
Pods
   ↓
Service
   ↓
Users
```

---

# Real-World Production Flow

```text
Internet
   ↓
Load Balancer
   ↓
Service
   ↓
Pods
   ↓
Application
```

---

# Important kubectl Commands Summary

| Command | Purpose |
|---|---|
| kubectl get svc | List Services |
| kubectl describe svc | Detailed Service info |
| kubectl get endpoints | Show endpoints |
| kubectl expose deployment | Create Service |
| kubectl delete svc | Delete Service |
| kubectl edit svc | Edit Service |
| kubectl get pods --show-labels | Show labels |
| kubectl logs | Check logs |
| kubectl port-forward | Local access |

---

# Recommended Practice

Practice:
- Creating ClusterIP Services
- Creating NodePort Services
- Label selectors
- Troubleshooting endpoints
- Service YAML creation

---

# Final Understanding

```text
Deployment creates Pods
Pods run Applications
Services expose Applications
```

Without Services:
- Pods are difficult to access

With Services:
- Stable networking becomes possible

---

# End of Kubernetes Services Notes
