Project 01: Secure Multi-Tier Private Infrastructure with Azure Internal Load Balancer
Business Scenario & Objective
Enterprise workloads processing internal business logic, databases, or microservices require strict perimeter isolation. Exposing these critical systems directly to the public internet creates unnecessary attack surfaces.

The objective of this project is to architect, deploy, and validate a secure, highly available multi-tier private architecture in Microsoft Azure. The backend application tier operates under complete network isolation with Zero Public Exposure, while an Azure Internal Load Balancer (Standard SKU) manages private traffic distribution and automated failover across backend nodes.

Architecture Overview
1. Network Segmentation & Micro-Isolation
Virtual Network (ilb-vnet): Provisioned with address space 10.70.0.0/16.

Frontend Subnet (frontend-subnet): 10.70.1.0/24 hosting a jump/test virtual machine (ilb-client-vm) to simulate internal consumer requests.

Backend Subnet (backend-subnet): 10.70.2.0/24 hosting the application instances (ilb-vm0 and ilb-vm1) alongside the load balancer endpoint.

2. Zero-Public-Exposure Backend Workloads
Both Windows Server instances (ilb-vm0, ilb-vm1) are deployed without Public IP addresses.

Workloads communicate exclusively inside the private virtual network fabric.

3. Layer-4 Private Load Balancing
Azure Internal Load Balancer (Standard SKU): Configured with a static private frontend IP address (10.70.2.4) in the backend subnet.

Load Balancing Rule: Directs incoming TCP Port 80 traffic evenly across instances in the backend pool.

TCP Health Probe: Continuously monitors TCP Port 80 at 5-second intervals with an unhealthy threshold of 2 consecutive failures.

4. Defense-in-Depth Access Hardening
Network Security Group (ilb-backend-nsg): Bound to the backend subnet.

Inbound Rule: Restricts HTTP ingress strictly to the VirtualNetwork service tag, dropping all traffic from outside the corporate virtual network.

Deployment & Validation Steps
1. Automated Web Server Provisioning
Both backend virtual machines were provisioned with IIS and unique identifiers using Azure Run Command:

Command executed on each backend node:
Install-WindowsFeature -name Web-Server -IncludeManagementTools
Set-Content -Path "C:\inetpub\wwwroot\Default.htm" -Value "Internal Tier: Response from $env:computername"

2. Traffic Distribution Test
From ilb-client-vm in the frontend subnet, an automated HTTP query loop was run targeting the load balancer private IP ([http://10.70.2.4](http://10.70.2.4)):

Result: Responses alternated between ILB-VM0 and ILB-VM1, confirming healthy round-robin Layer-4 SDN balancing.

3. Automated High Availability & Failover Test
Simulated an unexpected node failure by stopping ilb-vm0. The health probe detected the node degradation within 10 seconds:

Result: The Internal Load Balancer immediately rerouted 100% of internal queries to the remaining healthy instance (ilb-vm1) with zero connection timeouts or dropped packets.

Key Engineering Takeaways
Azure SDN vs. Traditional Hardware: Azure Internal Load Balancer does not act as an in-line proxy server; it operates directly within the Azure SDN fabric at line-rate speed without introducing compute bottlenecks.

Standard Load Balancer Probe IP: Health probes originate from the dedicated Azure infrastructure IP 168.63.129.16. Any custom NSG must allow probe traffic from this address.

Enterprise Zero-Trust Alignment: Isolating application logic into dedicated, non-routable subnets without public IPs aligns directly with the Azure Well-Architected Framework and Zero-Trust principles.

Tech Stack
Cloud Provider: Microsoft Azure

Networking: Azure Virtual Network, Subnets, Network Security Groups (NSGs), Service Tags

Load Balancing: Azure Internal Load Balancer (Standard SKU, Layer 4)

Compute: Azure Virtual Machines (Windows Server 2022)

Automation: PowerShell, Azure Run Command
