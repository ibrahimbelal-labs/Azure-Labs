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
