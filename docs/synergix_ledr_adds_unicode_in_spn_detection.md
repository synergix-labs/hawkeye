# Detecting AD Unicode Abuse with SYNERGIX LEDR and KQL

## Overview

The **SYNERGIX LEDR SIEM Integration Module** securely forwards **Windows Security Events** and **Sysmon telemetry** into an **Azure Log Analytics Workspace**, enabling advanced analytics and threat detection using **Kusto Query Language (KQL)**.

This integration allows defenders to detect modern **identity‑based Active Directory attacks**, including **Unicode manipulation of LDAP attributes** as observed in:

> **Unicode Manipulation: CVE‑2026‑25177 – Active Directory SPN Unicode Collision Privilege Escalation**

By leveraging KQL over normalized Windows event data, security teams can identify **stealthy account manipulation techniques** that evade traditional signature‑based monitoring.

---

**Key characteristics of the SYNERGIX LEDR integration:**

- Windows Security and Sysmon Event Forwarding
- No agent‑side event manipulation
- Full Unicode preservation in event payloads
- Azure‑native KQL analytics compatibility

This ensures forensic integrity while enabling advanced analytics directly on identity telemetry.

---

## Why Unicode Matters in Active Directory

Active Directory attributes such as **`servicePrincipalName` (SPN)** and **`userPrincipalName` (UPN)** are stored as **Unicode strings**.

Recent attacks—including exploitation of **CVE‑2026‑25177**—demonstrate how attackers can:

- Insert **zero‑width characters**
- Use **homoglyphs** (look‑alike characters)
- Leverage **full‑width Unicode punctuation**

These techniques allow attackers to:
- Bypass SPN uniqueness validation  
- Create visually identical but byte‑distinct SPNs  
- Trigger Kerberos mis‑issuance or authentication fallback  

This makes Unicode manipulation a **high‑risk identity signal**.

---

## Relevant MITRE ATT&CK Mapping

Unicode manipulation of AD attributes maps cleanly to multiple **MITRE ATT&CK techniques**, making it suitable for structured detection and reporting.

### Primary Technique

- **T1098 – Account Manipulation**  
  *Tactic:* Privilege Escalation, Persistence  
  Attackers directly modify identity attributes such as SPNs to alter authentication behavior.

### Supporting Techniques

- **T1036 – Masquerading**  
  Unicode homoglyphs and zero‑width characters cause malicious values to appear legitimate to analysts and tools.
  
- **T1558.003 – Kerberoasting**  
  Unicode‑altered SPNs can be abused to obtain Kerberos service tickets without obvious SPN enumeration.

- **T1078 – Valid Accounts**  
  Exploitation requires a legitimate domain account with delegated permissions.

These mappings allow Unicode abuse detections to be categorized, prioritized, and correlated within MITRE‑aligned workflows.

---

## Windows Security Events Available via SYNERGIX LEDR

The SYNERGIX LEDR module forwards **Directory Service Change Events** that are essential for detecting Unicode abuse, including:

- **Event ID 5136** – Directory Service Object Modified  
- **Event ID 5137** – Directory Object Created  
- **Event ID 4728 / 4732 / 4756** – Group Membership Changes

These events include attribute metadata such as:

- `AttributeLDAPDisplayName`
- `AttributeValue`
- `ObjectDN`
- `SubjectUserName`

When ingested into Azure Log Analytics, these fields become searchable and analyzable using KQL.

---

## KQL‑Based Detection Approach

Unicode abuse is best detected by identifying **non‑ASCII characters** within sensitive AD attributes.

A foundational detection pattern is:

```kql
matches regex @"[^\x00-\x7F]"
```
This expression matches any Unicode character outside the ASCII range, including:

- Zero‑width characters (U+200B, U+FEFF)
- Cyrillic and Greek homoglyphs
- Full‑width punctuation

### Example: Detect Unicode in SPN Modifications
The following query focuses on SPN changes detected via Event ID 5136:
```KQL

AppEvents
  | where Properties.EventId == 5136
  | extend AttributeValue_ = tostring(Properties.AttributeValue)
  | where Properties.AttributeLDAPDisplayName == "servicePrincipalName"
  | where Properties.AttributeValue matches regex @"[^\x00-\x7F]"
```
⚠️ Note:
> This query is intentionally stored in a separate .kql file within the repository for reuse, validation, and rule deployment.


Why This Detection Is High‑Value
In real‑world environments:

- Unicode characters are rarely used legitimately in SPNs
- Zero‑width characters are never valid operationally
- Attribute modification events are low‑volume but high‑impact

As a result, this detection has:

- Low false‑positive rates
- High investigative value
- Clear MITRE alignment
- Strong relevance to modern AD attack paths


Operational Use Cases
Using SYNERGIX LEDR + KQL, teams can:

- Detect stealthy SPN abuse that bypasses setspn.exe
- Identify identity‑based persistence mechanisms
- Correlate Unicode abuse with Kerberos ticket activity
- Support proactive hunting for CVE‑2026‑25177‑style techniques

This enables detection of attacks that do not involve malware, binaries, or exploit code—only identity manipulation.

## Summary
The combination of SYNERGIX LEDR, Windows Security event forwarding, and KQL analytics provides defenders with strong visibility into identity‑centric Active Directory attacks.
Unicode manipulation—as seen in CVE‑2026‑25177—is a prime example of why modern SIEM detection must go beyond simple string comparisons and leverage the full analytical power of KQL.
By monitoring directory service modifications and applying Unicode‑aware analytics, organizations can detect and stop privilege escalation attempts before domain dominance is achieved.
