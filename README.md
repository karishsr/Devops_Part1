# Microservice Stack Deployment - Part 1

## Overview

This project demonstrates a basic microservice stack deployed using Kubernetes. It includes three containerized services—gateway, auth-service, and data-service—managed with Helm. Each service has specific responsibilities and is deployed with best practices such as probes, resource limits, and ingress/network configurations.


## Setup Instructions

1. **Clone the repository**
   ```bash
   git clone <https://github.com/karishsr/Devops_Part1.git>
   cd microservice-stack
 
 **Architecture Diagram**
![image](https://github.com/user-attachments/assets/2e620996-9fe1-4a13-9a8a-8a57453f02c6)


**Design Decisions**
I used Helm to keep the configuration modular and reusable.

The gateway is deployed in the system namespace and exposed using ingress, while auth-service and data-service are internal-only in the app namespace.

I implemented liveness and readiness probes for better health checks.

I applied resource requests and limits to ensure balanced resource usage.

I configured NetworkPolicies to restrict access to the internal services only.

All services are deployed using publicly available Docker images.

**Security Incident & Fix**
Problem: Initially, all services were reachable from outside the cluster through ingress.

Solution: I restricted ingress access using NetworkPolicy, allowing only the gateway to be accessed publicly. Both auth-service and data-service are now protected and only receive internal traffic from the gateway.

**Assumptions & Known Issues**
I assume that the ingress controller is pre-installed (e.g., NGINX).

The services are stateless and do not use persistent volumes.

I have not modified the Docker images, so no custom Dockerfile is provided.

This is a mock setup; in real-world use, proper authentication and data logic would be added.
