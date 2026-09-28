# Project 01: Secure Multi-Tier Private Infrastructure with Azure Internal Load Balancer

## Business Scenario & Objective
Enterprise workloads processing internal business logic, databases, or microservices require strict perimeter isolation. Exposing these critical systems directly to the public internet creates unnecessary attack surfaces.

The objective of this project is to architect, deploy, and validate a secure, highly available multi-tier private architecture in Microsoft Azure. The backend application tier operates under complete network isolation with **Zero Public Exposure**, while an **Azure Internal Load Balancer (Standard SKU)** manages private traffic distribution and automated failover across backend nodes.

---

## Architecture Overview

### 1. Network Segmentation & Micro-Isolation
* **Virtual Network (`ilb-vnet`):** Provisioned with address space `10.70.0.0/16`.
* **Frontend Subnet (`frontend-subnet`):** `10.70.1.0/24` hosting a jump/test virtual machine (`ilb-client-vm`) to simulate internal consumer requests.
* **Backend Subnet (`backend-subnet`):** `10.70.2.0/24` hosting the application instances (`ilb-vm0` and `ilb-vm1`) alongside the load balancer endpoint.

### 2. Zero-Public-Exposure Backend Workloads
* Both Windows Server instances (`ilb-vm0`, `ilb-vm1`) are deployed without Public IP addresses.
* Workloads communicate exclusively inside the private virtual network fabric.

### 3. Layer-4 Private Load Balancing
* **Azure Internal Load Balancer (Standard SKU):** Configured with a static private frontend IP address (`10.70.2.4`) in the backend subnet.
* **Load Balancing Rule:** Directs incoming TCP Port 80 traffic evenly across instances in the backend pool.
* **TCP Health Probe:** Continuously monitors TCP Port 80 at 5-second intervals with an unhealthy threshold of 2 consecutive failures.

### 4. Defense-in-Depth Access Hardening
* **Network Security Group (`ilb-backend-nsg`):** Bound to the backend subnet.
* **Inbound Rule:** Restricts HTTP ingress strictly to the `VirtualNetwork` service tag, dropping all traffic from outside the corporate virtual network.

---

## Deployment & Validation Steps

### 1. Automated Web Server Provisioning
Both backend virtual machines were provisioned with IIS and unique identifiers using Azure Run Command:

