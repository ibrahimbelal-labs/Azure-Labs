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
  * **Access Verification:** Validated effective permissions across hierarchical scopes (Subscription vs. Resource Group) using the Azure IAM Check Access utility.
