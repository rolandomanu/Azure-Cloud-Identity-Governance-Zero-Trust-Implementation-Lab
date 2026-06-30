# Azure Cloud Identity Governance & Zero Trust Implementation Lab

## Executive Summary
This hands-on engineering lab demonstrates the implementation of Cloud Identity Governance, Role-Based Access Control (RBAC), and Zero Trust security principles within a Microsoft Entra ID environment. The project establishes a secure, enterprise-grade identity architecture by enforcing the Principle of Least Privilege (PoLP), strict Separation of Duties (SoD), and mandatory multi-factor authentication (MFA) baseline protections to safeguard corporate cloud infrastructure against credential-compromise vectors.

---

## 📊 Identity Architecture & RBAC Matrix
To systematically enforce the Principle of Least Privilege, corporate accounts were segregated into structural security groups mapping directly to specialized organizational roles. 

| Microsoft Entra ID Group | Test Identity | Assigned Directory Role | Resource Access Scope |
| :--- | :--- | :--- | :--- |
| **`Sec-IT-Admins`** | Carlos Mendoza | **Global Administrator** | **Full Tenant Control:** High-privilege administrative access over directory configurations. |
| **`Dept-HR`** | Alice Vance | **User (Standard)** | **Scoped Access:** Limited to human resources enterprise applications and department portals. |
| **`Dept-Finance`** | Elena Rojas | **User (Standard)** | **Scoped Access:** Restricted to financial databases and standard productivity tools. |
| **`Sec-External-Contractors`** | David Builder | **Guest / External User** | **Restricted Access:** Hardened profile for temporary external collaborators. |

---
## ⚙️ RBAC Implementation & Group Provisioning

To verify consistent access assignment, structural security groups were provisioned inside Microsoft Entra ID. This configuration ensures a decoupled architecture where users inherit permissions strictly through group governance rather than direct administrative assignment, mapping perfectly to the verification metrics seen in `image_90dabb.png`.

### 1. Administrative Group Infrastructure
* **Group Name:** `Sec-IT-Admins`
* **Assigned Directory Role:** `Global Administrator`
* **Impact:** Concentrates top-tier privileges into a single managed container, enforcing tight monitoring over administrative identities.
* **Evidence:** Documented in `screenshots/04-entra-id-groups.png`.

### 2. Departmental and Third-Party Segmentation
* **Group Name:** `Dept-Finance` / `Dept-HR`
* **Assigned Directory Role:** `None (Standard User Baseline)`
* **Impact:** Isolates departmental operations from tenant configurations, aligning with strict Separation of Duties (SoD).
* **Group Name:** `Sec-External-Contractors`
* **Assigned Directory Role:** `Guest Baseline`
* **Impact:** Isolates external identities to prevent over-privileged vertical scaling within the cloud infrastructure.

## 🛡️ Technical Validation & Multi-Factor Authentication (MFA)
By transitioning the tenant to an active defense posture via *Security Defaults*, explicit security controls were validated across distinct identity tiers:

### 1. Privileged Identity Enforcement (Administrative Tier)
* **Identity Tested:** Carlos Mendoza (`Sec-IT-Admins`)
* **Observed Control:** Attempting terminal login immediately triggered a hard stopping point, forcing the enrollment of a secondary verification factor. 
* **Evidence:** Documented in `screenshots/02-mfa-block-admin.png`.

### 2. Standard Identity Baseline (Operational Tier)
* **Identity Tested:** Elena Rojas (`Dept-Finance`)
* **Observed Control:** Consistent with an organization-wide Zero Trust framework, standard operational profiles are proactively challenged with a 14-day registration window for *Microsoft Authenticator* to eliminate weak password risks.
* **Evidence:** Documented in `screenshots/03-mfa-block-standard.png`.

---

## 📝 Module Conclusions
* **Effective Mitigation of Identity Risks via MFA:** The activation of native cloud security baselines proved to be an immediate, high-impact control for mitigating credential-compromise attacks. The tenant strictly enforces *Microsoft Authenticator* registration across all accounts, embedding a rigorous **Zero Trust** ("Never trust, always verify") posture.
* **Strict Alignment with the Principle of Least Privilege (Least Privilege):** Through proper Separation of Duties (SoD), standard operational accounts remain strictly restricted to their specific job scopes without granting tenant-level administrative capabilities, directly reducing the exposed blast radius in the event of a credential leak.
* **Reduction of Risk Exposure via Group Governance:** Consolidating users into structured, role-assigned groups ensures permissions are never provisioned loosely or individually, vastly simplifying compliance auditing, corporate governance, and the rapid revocation of access for temporary or external accounts.

---
## 🔍 Phase 4: Identity Auditing & Monitoring (Sign-in Logs)

### 1. Verification of Access Telemetry
To maintain continuous monitoring and support security operation workflows, Entra ID telemetry was audited to track administrative access and multi-factor authentication mechanics. 

* **Log Source:** Microsoft Entra ID ➡️ Monitoring ➡️ Sign-in logs.
* **Audit Profile Targeted:** Carlos Mendoza (`Sec-IT-Admins`)
* **Observed Event:** The authentication pipeline logged a successful initial credential validation followed by an immediate "Interrupted" or "MFA Required" status. This verifies that the defensive baseline effectively flags and holds privileged access requests until a strong second factor is provided.

### 2. SIEM & Log Analytics Integration Notes
In a production enterprise deployment, these raw sign-in and audit logs are continuously streamed via Diagnostic Settings to a **Log Analytics Workspace** or a cloud-native SIEM platform like **Microsoft Sentinel**. This framework enables:
* Automated alerting on anomalous or cross-region login attempts.
* Long-term log retention matching international security compliance frameworks (such as ISO 27001 and NIST CSF).
* Correlation of identity logs with cloud resource usage to detect privilege abuse or potential insider threats.

### 🛠️ Architecture Trade-offs & Baseline Selection
Due to licensing constraints preventing custom Conditional Access rule creation (Points 4 & 5 from `temp_image_B9A630BA-76A1-4AB8-A909-14C9C19F89C2.jpg`), the environment was deliberately transitioned to **Microsoft Security Defaults**. 

This deployment successfully accomplishes the following core security objectives:
1. **Enforces Modern Authentication:** Mandates multi-factor authentication for both administrative and standard user tiers.
2. **Hardens the Perimeter:** Automatically blocks all legacy authentication protocols (e.g., older email clients), removing common baseline exploit vectors.
3. **Contractor Security:** Establishes a uniform security posture where external identities are tightly bound to the same strict MFA enrollment workflow.
