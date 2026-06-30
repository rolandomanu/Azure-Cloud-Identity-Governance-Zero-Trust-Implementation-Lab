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
