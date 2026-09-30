# Microsoft Azure Administrator (AZ-104) - Hands-on Labs

A practical repository documenting hands-on labs, cloud infrastructure implementations, and automation scripts for the **AZ-104** certification.

---

## Completed Labs

### Lab 01: Manage Microsoft Entra ID Identities
* **Status:** Completed
* **Focus Areas:**
  * **User Lifecycle Management:** Created and managed cloud user identities (`az104-user1`, `az104-user2`) with precise enterprise profile attributes (Department, Job Title, Usage Location).
  * **Group Governance:** Configured assigned security groups (`IT Cloud Administrators`) for centralized access control and license assignment.
  * **Dynamic Membership Architecture:** Analyzed dynamic query criteria based on user directory attributes (`user.department -eq "IT"`).
  * **External Collaboration (B2B):** Managed guest lifecycle invitations, onboarding external consultants, and scoping security boundaries between Members and Guests.

---

### Lab 02: Manage Subscriptions and Governance (RBAC)
* **Status:** Completed
* **Focus Areas:**
  * **Scope Isolation:** Created dedicated resource groups (`az104-02-rg1`) to enforce granular boundary management.
  * **Built-in Role Assignment:** Assigned `Virtual Machine Contributor` to `az104-user1` at the resource group scope, enforcing the principle of least privilege.
  * **Custom Role Definition:** Authored and registered a custom RBAC role (`Custom Support Request Role`) leveraging fine-grained control actions (`Microsoft.Support/*`).
 
  ---
  * **Access Verification:** Validated effective permissions across hierarchical scopes (Subscription vs. Resource Group) using the Azure IAM Check Access utility.
### Lab 02b: Manage Governance via Azure Policy
* **Status:** Completed
* **Focus Areas:**
  * **Policy Assignment & Scoping:** Assigned the built-in `Allowed locations` policy scoped specifically to the `az104-02-rg1` resource group.
  * **Policy Enforcement Testing:** Validated prevention rules by attempting to deploy non-compliant resources in unauthorized regions and analyzing ARM rejection errors.
  * **Compliance Auditing:** Explored the Azure Policy Compliance dashboard to understand evaluation cycles, resource compliance states, and remediation lifecycles.
---

### Lab 03: Manage Azure Resources by Using the Azure Portal and ARM Templates
* **Status:** Completed
* **Focus Areas:**
  * **Resource Protection:** Configured and validated `Delete` resource locks to protect critical resource groups from accidental deletion.
  * **Resource Governance & Organization:** Applied environment and cost-allocation tags (`Environment: Dev`, `Department: IT`) and tracked them globally across the tenant.
  * **Infrastructure as Code (IaC):** Exported ARM templates into JSON format, analyzed template schema (`parameters`, `variables`, `resources`), and programmatically provisioned a managed disk (`az104-disk1`) via custom JSON template deployment.
---

### Lab 05: Implement Intersite Connectivity
* **Status:** Completed
* **Focus Areas:**
  * **Network Topology:** Designed a multi-region network layout comprising hub and spoke topologies across different address spaces (`10.50.0.0/16`, `10.51.0.0/16`, `10.52.0.0/16`).
  * **Local Peering:** Established bi-directional local virtual network peering between co-located VNets (`vnet0` and `vnet1` in East US).
  * **Global Peering:** Implemented cross-region global peering connecting workloads across geographically dispersed datacenters over the Microsoft global backbone.
  * **Routing Boundaries:** Analyzed traffic flow properties (Allow Gateway Transit, Forwarded Traffic) and validated non-transitive routing constraints inherent to cloud peering architectures.
---

### Lab 06 (Part 2): Implement Internal Load Balancer (Private Traffic Management)
* **Status:** Completed
* **Architecture:** Multi-tier isolated network architecture (`frontend-subnet` & `backend-subnet`).
* **Focus Areas:**
  * **Private Layer-4 Balancing:** Deployed an Internal Load Balancer (Standard SKU) with a dynamic private IP frontend (`10.70.2.x`) to completely eliminate direct internet exposure for application workloads.
  * **Network Security Isolation:** Configured Network Security Group (NSG) rules targeting the backend subnet to strictly allow intra-VNet HTTP traffic (`VirtualNetwork` service tag).
  * **Workload Provisioning & Probing:** Deployed two backend Windows Server instances, automated IIS configuration via Run Command, and monitored health using TCP Port 80 probes.
  * **Private Failover Validation:** Simulated node degradation and verified internal automated failover by issuing programmatic HTTP requests from an isolated client VM within the frontend tier.
# Enterprise Azure Storage Architecture: Lifecycle Automation, SAS Governance & Private Link Isolation

## Overview
This project implements an enterprise-grade, Zero-Trust Azure Storage architecture. It demonstrates secure data tiering, automated cost optimization via Lifecycle Management rules, fine-grained access control using Shared Access Signatures (SAS) and Stored Access Policies, and complete network isolation using Azure Private Link (Private Endpoints) and Service Endpoints.

---

## Architectural Diagram

```text
       [ Public Internet ]
               |
               X (Blocked by Storage Firewall)
               |
   +-----------V----------------------------------------------------+
   | Azure Virtual Network (vnet-storage-lab: 10.0.0.0/16)          |
   |                                                                |
   |  Subnet: default (10.0.0.0/24)                                 |
   |   - Service Endpoint: Microsoft.Storage                        |
   |   - Private Endpoint NIC (10.0.0.x)                            |
   |            |                                                   |
   |            +-------------------+                               |
   +--------------------------------|-------------------------------+
                                    | Private Link Connection
                                    |
   +--------------------------------V-------------------------------+
   | Azure Storage Account (stlab07azibra)                          |
   |                                                                |
   |  Private DNS Zone: privatelink.blob.core.windows.net          |
   |  (Resolves FQDN directly to 10.0.0.x private IP)               |
   |                                                                |
   |  Containers & Data Plane:                                      |
   |   - Container: data-reports (Private / No Anonymous Access)    |
   |   - Stored Access Policy: read-only-policy (rl)                |
   |   - Lifecycle Policy: Hot -> Cool (30d) -> Archive (90d) -> Del|
   +----------------------------------------------------------------+
# Azure IaaS: Virtual Machine Automation, Managed Disk Scaling & Cost Governance

## Overview
This project demonstrates the end-to-end administration and governance of Azure Virtual Machines (IaaS). It covers high-availability provisioning within Availability Zones, post-deployment automation via scripting, managed data disk lifecycle (attachment, host caching, online expansion), compute vertical scaling, and FinOps cost-control automation.

---

## Architecture Diagram

```text
  [ Azure Region ]
   |
   +-- [ Availability Zone 1 ] ------------------------------------+
       |                                                           |
       |  Virtual Network: vnet-az104-lab08 (10.0.0.0/16)          |
       |   - Subnet: default (10.0.0.0/24)                         |
       |   - Network Security Group (NSG): Inbound HTTP:80, RDP:3389|
       |                                                           |
       |  VM: vm-az104-web01                                       |
       |   - OS Disk: Standard SSD (Windows Server 2022)           |
       |   - Data Disk: Standard SSD (Scaled 32GB -> 64GB)         |
       |     * Host Caching: Read/Write                            |
       |     * Partition: GPT / NTFS (F: Drive)                    |
       |                                                           |
       |  Post-Provisioning & Automation:                          |
       |   - IIS Web Server installation via Run Command / Ext     |
       |   - Auto-Shutdown Policy: 19:00 (UTC+3) Daily             |
       +-----------------------------------------------------------+
