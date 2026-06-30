# Lab 02b: Manage Governance via Azure Policy

![Azure](https://img.shields.io/badge/Microsoft-Azure-0078D4?logo=microsoftazure&logoColor=white)
![Policy](https://img.shields.io/badge/Azure-Policy-blue)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

[⬅ Back to AZ-104 series overview](../README.md)

## 📋 Overview

This folder documents my hands-on lab work for **AZ-104 (Microsoft Azure Administrator)** — Lab 02b: *Manage Governance via Azure Policy*. The goal of this lab was to implement organizational governance using tags, Azure Policy, and resource locks, including:

- Applying resource tags via the Azure portal
- Enforcing tagging on new resources using Azure Policy
- Applying tagging to existing resources via policy remediation
- Configuring and testing resource locks

## 🎯 Lab Scenario

> Our organization's cloud footprint grew significantly, and an audit found many resources without a defined owner, project, or cost center. To fix this, I implemented resource tagging, enforced tagging via Azure Policy, remediated existing resources, and applied resource locks to protect configured resources.

## 🛠️ Tools Used

- Microsoft Azure Portal
- Azure Policy
- Azure Resource Groups / Tags / Locks
- Region: East US

---

## 🧭 Step-by-Step Walkthrough

### Task 1: Assign Tags via the Azure Portal

#### Step 1 — Create a resource group with a tag
Created a new resource group with a Cost Center tag.

| Field | Value |
|---|---|
| Resource group name | az104-rg2 |
| Region | East US |
| Tag name | Cost Center |
| Tag value | 000 |

📸 **Screenshot:** `01-create-rg-tags.png` — capture the Tags tab filled in during resource group creation (before clicking Review + Create).

![Create RG with Tags](./screenshots/01-create-rg-tags.png)

📸 **Screenshot:** `02-rg-created.png` — capture the resource group Overview confirming `az104-rg2` exists with the Cost Center tag visible.

![Resource Group Created](./screenshots/02-rg-created.png)

---

### Task 2: Enforce Tagging via an Azure Policy

#### Step 1 — Browse policy definitions
Opened Policy → Authoring → Definitions, and searched for the built-in policy.

📸 **Screenshot:** `03-policy-definitions.png` — capture the Definitions blade showing built-in policy definitions, with "Require a tag and its value on resources" visible in search.

![Policy Definitions](./screenshots/03-policy-definitions.png)

#### Step 2 — Assign the "Require a tag and its value" policy
Assigned the policy scoped to `az104-rg2` with:

| Field | Value |
|---|---|
| Assignment name | Require Cost Center tag and its value on resources |
| Policy enforcement | Enabled |
| Tag Name | Cost Center |
| Tag Value | 000 |

📸 **Screenshot:** `04-assign-policy-basics.png` — capture the Basics tab of the policy assignment filled in.

![Assign Policy Basics](./screenshots/04-assign-policy-basics.png)

📸 **Screenshot:** `05-assign-policy-parameters.png` — capture the Parameters tab showing Tag Name and Tag Value set.

![Assign Policy Parameters](./screenshots/05-assign-policy-parameters.png)

#### Step 3 — Test enforcement by creating a non-compliant storage account
Attempted to create a Storage Account in `az104-rg2` without the required tag.

📸 **Screenshot:** `06-policy-validation-failed.png` — capture the "Validation failed" error confirming deployment was disallowed by the policy.

![Policy Validation Failed](./screenshots/06-policy-validation-failed.png)

---

### Task 3: Apply Tagging via an Azure Policy (Remediation)

#### Step 1 — Delete the original policy assignment
Removed the "Require a tag" policy assignment to switch strategies.

📸 **Screenshot:** `07-delete-policy-assignment.png` — capture the delete confirmation for the original policy assignment.

![Delete Policy Assignment](./screenshots/07-delete-policy-assignment.png)

#### Step 2 — Assign the "Inherit a tag from the resource group if missing" policy
Configured a new assignment with remediation enabled:

| Field | Value |
|---|---|
| Assignment name | Inherit the Cost Center tag and its value 000 from the resource group if missing |
| Policy enforcement | Enabled |
| Tag Name | Cost Center |
| Create a remediation task | Enabled |

📸 **Screenshot:** `08-inherit-tag-policy.png` — capture the new policy assignment's Basics/Parameters showing the Inherit tag policy configured.

![Inherit Tag Policy](./screenshots/08-inherit-tag-policy.png)

📸 **Screenshot:** `09-remediation-task.png` — capture the Remediation tab showing the remediation task enabled.

![Remediation Task](./screenshots/09-remediation-task.png)

#### Step 3 — Verify the policy auto-tags a new resource
Created another storage account in `az104-rg2` without manually adding the tag.

📸 **Screenshot:** `10-storage-validation-passed.png` — capture confirmation that validation passed this time.

![Storage Validation Passed](./screenshots/10-storage-validation-passed.png)

📸 **Screenshot:** `11-tag-auto-applied.png` — capture the new storage account's Tags blade showing Cost Center = 000 was automatically applied.

![Tag Auto Applied](./screenshots/11-tag-auto-applied.png)

---

### Task 4: Configure and Test Resource Locks

#### Step 1 — Add a delete lock
On `az104-rg2` → Settings → Locks → Add.

| Field | Value |
|---|---|
| Lock name | rg-lock |
| Lock type | Delete |

📸 **Screenshot:** `12-add-resource-lock.png` — capture the Add lock panel filled in before clicking OK.

![Add Resource Lock](./screenshots/12-add-resource-lock.png)

#### Step 2 — Test the lock by attempting to delete the resource group
Attempted to delete `az104-rg2` from its Overview blade.

📸 **Screenshot:** `13-delete-denied.png` — capture the notification denying the deletion due to the active lock.

![Delete Denied](./screenshots/13-delete-denied.png)

---

## 🧹 Cleanup

After completing the lab: removed the resource lock, then deleted the `az104-rg2` resource group to free up resources.

---

## ⚠️ Challenges Encountered

*(to be filled in — keep notes in `challenges.md` as you go)*

## 💡 Key Takeaways

- **Tags** are key-value metadata pairs used to label resources logically (owner, cost center, project, etc.).
- **Azure Policy** enforces organizational conventions — definitions describe compliance conditions and the effect to take if violated.
- Built-in policies like **"Require a tag and its value on resources"** block non-compliant deployments outright.
- Policies with a **Modify** effect (like "Inherit a tag...") require a **managed identity** and a **remediation task** to fix existing non-compliant resources.
- **Resource locks** (Delete or Read-only) protect resources from accidental deletion or modification, and override user permissions.
- **Azure Policy is a pre-deployment control; RBAC and resource locks are post-deployment controls.**

## 🔗 Related Resources

- [Azure Policy documentation](https://learn.microsoft.com/azure/governance/policy/overview)
- [AZ-104 Certification path](https://learn.microsoft.com/credentials/certifications/azure-administrator/)

---

[⬅ Back to AZ-104 series overview](../README.md)
