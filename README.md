**Parts 1, 3, 4 & 5**
 
This repository contains my implementation of a secure, observable microservices stack deployed using Kubernetes. The stack is structured around real-world challenges including observability, security enforcement, and fault tolerance.

## Part 1: Microservice Stack Deployment

### Services Deployed

| Service       | Purpose                          | Docker Image             | Namespace |
|---------------|----------------------------------|---------------------------|-----------|
| `gateway`     | API gateway exposed via Ingress  | `nginxdemos/hello`        | `system`  |
| `auth-service`| Auth logic (logs headers)        | `kennethreitz/httpbin`    | `app`     |
| `data-service`| Mock business logic              | `hashicorp/http-echo`     | `app`     |

### Features

- **Deployed with Helm**
- **Readiness/Liveness Probes**
- **Resource requests and limits set**
- **Ingress configured for `gateway`**
- **auth-service and data-service kept internal**
- **Namespaces:**
  - `system` for infrastructure components
  - `app` for internal services

## Part 3: Security Incident Simulation

### Scenario

I discovered that the `auth-service` was leaking sensitive `Authorization` headers in logs. Additionally, it had unrestricted egress, potentially accessing external resources.

### Investigation

Used:
- `kubectl logs`
- Curling `/headers` endpoint of httpbin

### Fixes Applied

- Removed sensitive headers from logs using proper HTTP headers filtering
- Applied **NetworkPolicies**:
  - `auth-service` can only talk to `data-service`
  - All other egress blocked
- Implemented **OPA** policy to block pods that log Authorization headers

---

## Part 4: Observability

### Stack Deployed

- Installed **Prometheus & Grafana** using Helm:
  ```bash
  helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
  helm install monitoring prometheus-community/kube-prometheus-stack

**Part 5: Failure Simulation**
**Simulated Failures**
Manually deleted a pod using kubectl delete pod

Applied node pressure using stress pod

**System Behavior**
Auto-recovery via ReplicaSet

Resource alerts visible in Grafana

Pod replacement verified using kubectl get pods -w

**Architecture Design**
![image](https://github.com/user-attachments/assets/e2037206-6d60-4648-a63f-afa38ab5377d)

**Design Decisions**
Chose httpbin for easy header inspection during security sim

Enforced least privilege via NetworkPolicies

Separated infra (system) and services (app) using namespaces

Used Prometheus stack for comprehensive metrics collection

**Known Issues or Assumptions**
In real scenarios, custom apps should avoid logging sensitive data.

No persistent volumes were used to keep this simulation lightweight.

Grafana alerts need manual SMTP or webhook configuration to notify.


