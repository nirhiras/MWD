# Enterprise Cybersecurity Strategy & Roadmap
### Prepared for: MWD (Water Utility / Critical Infrastructure — Water & Wastewater Systems Sector)
**Prepared by:** Office of the CISO (Strategy Document)
**Date:** July 2026
**Classification:** Internal Use Only — Security Sensitive

---

## How to Read This Document

This strategy treats MWD as a **critical infrastructure operator** first and an 1,800-person enterprise second. That framing drives several recommendations that a generic IT security assessment would miss: OT/ICS (SCADA) protection for treatment and distribution systems, alignment with the **America's Water Infrastructure Act (AWIA) §2013** risk and resilience assessment cycle, CISA's **Water and Wastewater Systems Sector** guidance, WaterISAC threat sharing, and NIST SP 800-82 (ICS security) alongside the requested NIST SP 800-53 Rev. 5 and CIS Controls v8 mappings.

The existing team — CISO, Cybersecurity Manager, 3 Threat Analysts, 1 Network Specialist, 3 GRC Consultants (9 FTE total) — is **compliance-heavy and operations-light**. 3 of 9 FTEs (33%) are dedicated to compliance/control assessment, while only 3 analysts and 1 network engineer carry detection, response, hunting, and network security operations combined. No OT/ICS security engineer, no dedicated IAM/PAM administrator, no vulnerability management analyst, and no 24x7 coverage exist today. This imbalance is the single most important structural finding in this report and shapes the staffing and managed-services recommendations throughout.

---

## 1. Executive Summary

### 1.1 Current Security Posture

MWD has invested in a credible foundation of enterprise-grade tooling: CrowdStrike Falcon (EDR), IBM QRadar (SIEM), Palo Alto Panorama (NGFW management), Cisco ISE (NAC), Cisco Umbrella (DNS security), Zscaler ZPA/VPN (remote access), Tanium (endpoint visibility/management), Centrify/Delinea (PAM), VMware Horizon (VDI), Microsoft 365, and SolarWinds (network monitoring). This is a mid-to-upper maturity **tooling** stack. However, tooling maturity is running ahead of **process, staffing, and OT coverage** maturity.

### 1.2 Strengths

- **Strong endpoint and network security tooling**: CrowdStrike + Tanium give strong endpoint telemetry and control; Panorama + ISE + Umbrella give layered network/DNS control.
- **Centralized SIEM** (QRadar) already exists as an aggregation point for correlation and compliance evidence.
- **PAM foundation** in place (Centrify/Delinea) rather than being built from zero.
- **Dedicated GRC capacity** (3 consultants) — a rarity at this org size — gives MWD a real head start on continuous NIST 800-53 / CIS control assessment rather than annual scramble audits.
- **Zero Trust remote access foundation** via Zscaler ZPA reduces flat-VPN exposure already.

### 1.3 Key Risks

| Risk | Why It Matters |
|---|---|
| **No dedicated OT/ICS security monitoring** | MWD operates SCADA/ICS for treatment and distribution. IT-grade tools (CrowdStrike, QRadar) have little to no visibility into PLCs, RTUs, HMIs, or OT protocols (Modbus, DNP3, EtherNet/IP). Ransomware or nation-state actors targeting IT/OT boundaries is the #1 sector threat per CISA/WaterISAC advisories. |
| **No 24x7 detection & response coverage** | 3 Threat Analysts cannot cover 24/7/365. Dwell time increases materially outside business hours — the window attackers already prefer. |
| **No dedicated incident response function** | Threat Analysts, Network Specialist, and Cybersecurity Manager are pulled into IR ad hoc; no named IR commander rotation or retained forensics/IR firm. |
| **No enterprise vulnerability management platform** | Tanium provides asset/patch visibility but is not a substitute for a dedicated, risk-scored VM program (CVSS + EPSS + asset criticality). |
| **No dedicated email security layer beyond M365 native** | M365 EOP/Defender for Office (base) is insufficient against BEC and credential-phishing given water-utility targeting by ransomware affiliates. |
| **No CASB/SSPM or CNAPP/CSPM** | Cloud/SaaS posture (M365 and any IaaS) is effectively unmonitored for misconfiguration and shadow SaaS. |
| **SolarWinds as network monitoring platform** | Post-SUNBURST (2020), SolarWinds requires explicit supply-chain risk management (patch attestation, network segmentation of the management plane, no direct internet egress) — not a reason to replace it, but a reason to formally govern it. |
| **No formal secrets management or PKI/certificate lifecycle tooling** | Increases risk of hardcoded credentials and certificate-expiration outages, both common root causes of unplanned downtime in ICS-adjacent environments. |
| **Compliance-heavy, operations-light staffing** | 3 GRC consultants vs. 3 analysts + 1 network engineer for detection/response/network ops. No architecture, IAM, or OT security headcount at all. |

### 1.4 Security Maturity (Composite, NIST CSF-style scoring, 1–5)

| Function | Maturity | Notes |
|---|---|---|
| Identify | 3.0 | GRC capacity is strong; asset inventory for OT/ICS is weak |
| Protect | 3.2 | Strong endpoint/network controls; gaps in DLP, email, PAM depth |
| Detect | 2.5 | SIEM exists but under-resourced for 24x7 correlation/hunting; no OT detection |
| Respond | 2.0 | No formal IR retainer, no SOAR, ad hoc roles |
| Recover | 2.3 | Backup exists (implied via SolarWinds/infra) but immutability/testing not confirmed |
| **Overall** | **~2.6 / 5 (Developing → Defined)** | Tooling is ahead of process; OT and 24x7 ops are the biggest maturity drags |

### 1.5 Strategic Priorities (Next 12 Months)

1. **Close the OT/ICS visibility gap** — this is the top sector-specific risk and the top board-level risk.
2. **Achieve 24x7 detection & response coverage** via MDR augmentation of existing CrowdStrike/QRadar investment (fastest path — no rip-and-replace).
3. **Stand up a real vulnerability management program** with SLA-driven remediation, not just patch visibility.
4. **Close identity gaps**: phishing-resistant MFA everywhere, PAM depth (session recording, JIT), secrets management.
5. **Operationalize the GRC investment** into continuous control monitoring rather than point-in-time assessment, using the SIEM/CSPM/CNAPP as evidence sources.
6. **Formalize IR, backup immutability, and DR testing** — assume ransomware is the primary scenario to defend against.

### 1.6 Top 5 Recommendations

1. Contract **MDR/24x7 SOC augmentation** (leverage CrowdStrike Falcon Complete/OverWatch, already licensed vendor) — fastest, lowest-disruption path to close the coverage gap.
2. Deploy **OT/ICS security monitoring** (Claroty, Dragos, or Nozomi Networks) at all treatment/pumping/distribution facilities — required for AWIA risk-and-resilience posture and sector threat reality.
3. Deploy a **dedicated vulnerability management platform** (Tenable or Rapid7) with SLA-based remediation tracked as a KPI.
4. Deploy **SOAR** integrated with QRadar to multiply the effective capacity of 3 analysts through automation.
5. Formalize **backup immutability and ransomware recovery testing** (air-gapped/immutable copies, quarterly restore tests).

---

## 2. Gap Assessment

### 2.1 Technology Stack Coverage Matrix

| Security Capability | Current State | Gap |
|---|---|---|
| Endpoint Protection (EDR) | CrowdStrike Falcon | Adequate — extend to 24x7 managed response (OverWatch/Falcon Complete) |
| SIEM | IBM QRadar | Adequate platform; under-resourced for tuning/content development and 24x7 monitoring |
| Network Firewall Mgmt | Palo Alto Panorama | Adequate — formal rule review cadence needed |
| NAC | Cisco ISE | Adequate — extend to full 802.1X + OT network segmentation enforcement |
| DNS/Web Security | Cisco Umbrella | Adequate |
| Remote Access / ZTNA | Zscaler ZPA/VPN | Adequate — retire remaining legacy VPN in favor of full ZTNA |
| Endpoint/Asset Mgmt | Tanium | Strong for IT; **no OT/ICS asset inventory** |
| PAM | Centrify/Delinea | Foundation present — depth gap (session recording, JIT/JEA, secrets vaulting) |
| VDI | VMware Horizon | Adequate |
| Productivity/Email | Microsoft 365 | **Native email security insufficient**; Entra ID licensing tier needs confirmation for Conditional Access/Identity Protection |
| Network Monitoring | SolarWinds | Adequate for NPM; **supply-chain governance gap** (patch attestation, segmentation) |
| **OT/ICS Security** | **None identified** | **Critical gap** — no ICS-aware IDS, asset inventory, or protocol-level monitoring |
| **SOAR** | None | High gap — analyst capacity multiplier missing |
| **Threat Intelligence Platform** | Ad hoc | Medium gap — no structured TIP or feed curation beyond vendor defaults |
| **Vulnerability Management** | Tanium (partial) | High gap — no dedicated, risk-prioritized VM platform |
| **Email Security (advanced)** | M365 native only | High gap — BEC/phishing is the #1 initial access vector |
| **CASB/SSPM** | None | Medium-high gap — no visibility into SaaS posture/shadow IT |
| **CNAPP/CSPM** | None | Medium-high gap (scales with any cloud/IaaS footprint) |
| **DLP** | None dedicated (M365 Purview unconfirmed) | High gap for a utility handling PII, OT design data, SCADA network diagrams |
| **Secrets Management** | None | Medium-high gap |
| **PKI / Certificate Lifecycle** | None | Medium gap — cert-expiry outages are a common self-inflicted risk |
| **Attack Surface Management** | None | Medium gap — no continuous external attack surface visibility |
| **Security Awareness Training** | Unconfirmed/ad hoc | Medium gap |
| **Backup Immutability/Ransomware Recovery** | Unconfirmed | High gap given ransomware targeting of utilities |
| **Asset Discovery (OT)** | None | Critical gap (ties to OT/ICS finding above) |

### 2.2 Operational Gaps

- No 24x7/on-call detection coverage.
- No formal IR retainer with outside forensics/negotiation counsel.
- No SOAR — response time is bound by manual analyst throughput.
- No dedicated vulnerability management analyst role — VM is implicitly split across Threat Analysts/Network Specialist.
- No OT/ICS security engineer or program owner.
- No IAM/PAM administrator role distinct from the Network Specialist/Cybersecurity Manager.
- No security architecture function (architecture decisions currently default to whoever is available).

### 2.3 Compliance Gaps (Preview — full mapping in Section 11)

- **AC (Access Control)**: Partially met — PAM exists but least-privilege/JIT and periodic access recertification are not evidenced as formal, recurring processes.
- **CP (Contingency Planning)**: Partially met — backup exists but immutability and full-scale DR testing are unconfirmed.
- **IR (Incident Response)**: Partially met — capability exists informally but lacks a documented, tested IR plan with defined roles (Section 6 fixes this).
- **RA (Risk Assessment)**: Partially met — 3 GRC consultants perform assessments, but no continuous vulnerability scanning/risk-scoring feeds this.
- **SC (System & Communications Protection)**: Partially met for IT; **not met** for OT network boundary enforcement and monitoring.
- **SI (System & Information Integrity)**: Partially met — no dedicated FIM or ICS anomaly detection.
- **SR (Supply Chain Risk Management)**: Not formally addressed (SolarWinds governance, OT vendor/integrator risk).

### 2.4 High-Risk Areas (Ranked)

1. OT/ICS visibility and segmentation.
2. Off-hours detection/response coverage (nights, weekends, holidays).
3. Ransomware resilience (backup immutability + tested recovery).
4. Initial-access vectors: phishing/BEC (email security gap) and unmanaged SaaS (CASB/SSPM gap).
5. Vulnerability remediation velocity (no SLA-driven program).
6. Third-party/OT vendor and SolarWinds supply-chain exposure.

---

## 3. Recommended Security Architecture

Design principles: **Zero Trust**, **Defense in Depth**, **Least Privilege**, **Identity-First Security**, **Continuous Monitoring** — extended to explicitly cover the **IT/OT boundary**, which is the architectural feature most enterprise security architectures omit and which matters most for MWD.

### 3.1 Identity
- Entra ID (M365) as the identity backbone; confirm **P2 licensing** for Identity Protection + risk-based Conditional Access.
- Phishing-resistant MFA (FIDO2/WebAuthn) for all privileged and remote-access accounts; app-based MFA minimum for standard users.
- Centrify/Delinea PAM extended to: session recording for all privileged sessions, just-in-time (JIT) elevation, vaulting of all shared/service accounts, and rotation automation.
- Conditional Access policies enforced at Zscaler ZPA and M365 boundary (device compliance + user risk + location).
- Quarterly access recertification for privileged and sensitive-data-access roles (owned by GRC consultants, executed via IAM tooling — see Section 5).

### 3.2 Endpoints (IT)
- CrowdStrike Falcon retained as EDR of record; add **Falcon Complete or OverWatch** for managed 24x7 detection/response and proactive threat hunting.
- Tanium retained for real-time asset inventory, patch enforcement, and configuration compliance (CIS benchmarks).
- Application allowlisting evaluated for high-risk endpoints (HMI workstations, engineering laptops) — see OT section.

### 3.3 Servers
- CrowdStrike Falcon extended to all server workloads (Windows/Linux, physical and virtual).
- Tanium for patch/configuration compliance against CIS Benchmarks.
- File Integrity Monitoring (FIM) enabled on servers handling regulated data and control-system historian/engineering servers.
- Server tiering (Tier 0/1/2 administrative model) enforced through PAM.

### 3.4 OT/ICS (SCADA) — New Domain
- Deploy a **passive, non-disruptive OT network monitoring platform** (Claroty, Dragos, or Nozomi Networks) at all treatment plants, pump stations, and distribution control points to build an authoritative OT asset inventory and detect anomalous protocol behavior (Modbus/DNP3/EtherNet-IP).
- Enforce a hard **IT/OT network segmentation boundary** using existing Palo Alto Panorama + Cisco ISE (unidirectional gateways or tightly ruled firewalls at the Purdue Model Level 3/4 boundary); no direct OT-to-internet egress.
- Feed OT alerts into QRadar via the OT platform's SIEM connector so the existing SOC (once augmented — see Section 5) gains OT visibility without a second console for Tier-1 triage.
- Align this workstream explicitly to the **AWIA §2013 risk and resilience assessment** cycle and CISA Water Sector guidance.

### 3.5 Cloud
- Deploy **CNAPP/CSPM** (e.g., Microsoft Defender for Cloud if Azure-resident, or Wiz/Prisma Cloud if multi-cloud) for misconfiguration detection, workload protection, and IaC scanning.
- Enforce baseline hardening via CIS Cloud Benchmarks.

### 3.6 Network
- Palo Alto Panorama-managed NGFWs as the enforcement point for all inter-zone traffic, including the new IT/OT boundary.
- Cisco ISE extended to full 802.1X + dynamic segmentation (TrustSec) for both corporate and OT-adjacent network zones.
- Cisco Umbrella retained for DNS-layer protection; add DNS logging as a mandatory SIEM feed if not already integrated.
- SolarWinds governed under a formal supply-chain risk process: management-plane network isolation, patch attestation before deployment, no direct internet exposure of the SolarWinds server.

### 3.7 Email
- Deploy a dedicated advanced email security layer (e.g., Proofpoint, Mimecast, or Abnormal Security) in front of/integrated with M365 for BEC detection, URL/attachment sandboxing, and impersonation protection — layered on top of, not replacing, M365 Defender for Office.

### 3.8 Remote Access
- Zscaler ZPA as the standard for all remote application access (ZTNA model — no network-level VPN trust).
- Legacy site-to-site or user VPN reduced to the minimum required for specific OT vendor remote-support use cases, with time-bound, brokered access via PAM (no standing VPN credentials for vendors).

### 3.9 Data Protection
- Deploy **DLP** (Microsoft Purview DLP, leveraging existing M365 investment) for email, SharePoint, Teams, and endpoint egress channels; extend policies to cover OT engineering drawings/SCADA network diagrams as a sensitive data category.
- Deploy **CASB/SSPM** to monitor SaaS configuration drift and discover shadow SaaS.
- Deploy **secrets management** (HashiCorp Vault or CyberArk Conjur) for application/service credentials and API keys, integrated with the PAM platform.
- Deploy **PKI/certificate lifecycle management** (e.g., Venafi or a managed Microsoft PKI + monitoring) to eliminate expiry-driven outages and enforce internal cert hygiene.

### 3.10 Logging
- QRadar remains the SIEM of record; expand log source coverage to include OT platform alerts, CASB/CNAPP findings, DLP events, and email security verdicts.
- Centralize retention policy aligned to NIST AU family and any state/AWIA record-retention requirements.

### 3.11 Security Operations
- SOC model: existing 3 Threat Analysts remain Tier 2/3 (escalation, hunting, OT triage) while an **MDR provider covers Tier 1 and 24x7 monitoring**, closing the coverage gap without a large hiring cycle.
- SOAR deployed to automate Tier 1 triage/enrichment playbooks (phishing report handling, EDR isolation, IOC blocklisting), directly multiplying analyst throughput.
- Threat intelligence curated through WaterISAC (sector-specific) plus a commercial feed integrated into QRadar/SOAR.

### 3.12 Disaster Recovery & Backup Security
- Confirm/implement **3-2-1-1 backup strategy** (3 copies, 2 media types, 1 offsite, 1 immutable/air-gapped) for all critical systems including OT engineering/historian data.
- Immutable backup storage (e.g., object-lock enabled repository) explicitly scoped as ransomware recovery insurance.
- Quarterly restore testing with results reported as a KPI (Section 8) and validated in tabletop/DR exercises (Section 5).

---

## 4. Missing Products & Licensing Recommendations

Only genuinely missing capabilities are listed — nothing here duplicates existing CrowdStrike/QRadar/Panorama/ISE/Umbrella/Zscaler/Tanium/Centrify/Horizon/M365/SolarWinds functionality.

| # | Product Category | Example Vendor(s) | Purpose | Why Needed | Priority | Licensing Model (Est.) | Phase |
|---|---|---|---|---|---|---|---|
| 1 | OT/ICS Security Monitoring | Claroty, Dragos, Nozomi Networks | Passive OT asset inventory + anomaly/threat detection | #1 sector risk; no current OT visibility; AWIA alignment | **Critical** | Per-site/sensor subscription (~$150–400K/yr for a multi-facility water utility) | Phase 1 (Months 1–3 assess, 4–6 deploy) |
| 2 | Managed Detection & Response (24x7) | CrowdStrike Falcon Complete/OverWatch | Closes 24x7 coverage gap using existing EDR investment | 3 analysts cannot cover 24/7/365; fastest path to coverage | **Critical** | Per-endpoint annual subscription | Phase 1 (Months 1–2) |
| 3 | Vulnerability Management Platform | Tenable.io / Tenable.sc, Rapid7 InsightVM | Risk-prioritized, SLA-driven vulnerability management | No dedicated VM tool today; Tanium ≠ full VM program | **Critical** | Per-asset annual subscription | Phase 1 (Months 2–3) |
| 4 | SOAR | Palo Alto Cortex XSOAR, IBM QRadar SOAR | Automate Tier-1 triage/response playbooks | Multiplies analyst capacity; enables consistent IR execution | **High** | Per-analyst/user annual subscription | Phase 2 (Months 4–5) |
| 5 | Advanced Email Security | Proofpoint, Mimecast, Abnormal Security | Anti-phishing/BEC/impersonation protection layered on M365 | #1 initial-access vector not adequately covered natively | **High** | Per-mailbox annual subscription | Phase 1 (Months 2–3) |
| 6 | DLP | Microsoft Purview DLP (leverages existing M365 E5 if licensed) | Prevent exfiltration of PII/OT design data | No enterprise DLP today | **High** | Included in M365 E5 or add-on per-user | Phase 1 (Months 2–3) |
| 7 | CASB / SSPM | Microsoft Defender for Cloud Apps, Netskope | SaaS posture monitoring, shadow IT discovery | No SaaS security visibility today | **High** | Per-user annual subscription | Phase 2 (Months 4–5) |
| 8 | CNAPP / CSPM | Microsoft Defender for Cloud, Wiz, Prisma Cloud | Cloud misconfiguration & workload protection | No cloud posture tool today | **Medium-High** | Per-workload/resource annual subscription | Phase 3 (Months 7–8) |
| 9 | Threat Intelligence Platform | Recorded Future, Anomali, WaterISAC feed integration | Structured, sector-relevant threat intel curation | Currently ad hoc; sector-specific intel (WaterISAC) underused | **Medium-High** | Per-analyst annual subscription | Phase 2 (Months 4–5) |
| 10 | Attack Surface Management | CyCognito, IBM Randori, Censys | Continuous external attack surface discovery | No outside-in visibility today | **Medium** | Flat annual subscription | Phase 2 (Months 5–6) |
| 11 | Secrets Management | HashiCorp Vault, CyberArk Conjur | Eliminate hardcoded credentials/API keys | No secrets vaulting today; complements Centrify/Delinea PAM | **Medium-High** | Per-node/secret annual subscription | Phase 1 (Months 3–4) |
| 12 | PKI / Certificate Lifecycle Management | Venafi, Microsoft PKI + monitoring | Prevent cert-expiry outages; enforce cert hygiene | No lifecycle tooling today | **Medium** | Per-certificate/annual subscription | Phase 3 (Months 7–8) |
| 13 | Security Awareness Training Platform | KnowBe4, Proofpoint Security Awareness | Phishing simulation + training | Ad hoc/unconfirmed program today | **High** | Per-user annual subscription | Phase 1 (Month 1) |
| 14 | Immutable Backup / Ransomware Recovery | Veeam (with immutable/object-lock repository), Rubrik | Guaranteed clean recovery copy | Backup immutability unconfirmed; ransomware is top scenario | **Critical** | Per-TB/capacity annual subscription | Phase 1 (Months 1–2) |
| 15 | IT/OT Asset Discovery (if not bundled with #1) | Included in Claroty/Dragos/Nozomi platform | Authoritative OT asset inventory | Feeds risk assessment, vulnerability management, AWIA reporting | **Critical** | Bundled with item #1 | Phase 1 |

---

## 5. Security Operations Plan

### 5.1 Daily Activities

| Activity | Owner |
|---|---|
| SIEM alert monitoring & triage (Tier 1, augmented hours) | MDR Provider (Tier 1) / Threat Analysts (business hours escalation) |
| Endpoint (CrowdStrike) alert review & isolation actions | Threat Analysts |
| Incident triage & initial classification | Threat Analysts (on-call rotation) |
| Threat intelligence feed review (WaterISAC + commercial TIP) | Threat Analysts |
| PAM session/vault activity review (privileged logins, anomalies) | Cybersecurity Manager |
| Firewall/Panorama policy hit and deny-log review | Network Specialist |
| OT/ICS anomaly alert review (once deployed) | Threat Analysts (rotating OT-trained analyst) |
| DNS/Umbrella blocked-request review | Network Specialist |
| Email security quarantine/BEC alert review | Threat Analysts |
| Vulnerability scanner exception/false-positive triage | Cybersecurity Manager (until dedicated VM analyst hired) |

### 5.2 Weekly Activities

| Activity | Owner |
|---|---|
| Vulnerability scan review & remediation tracking | Cybersecurity Manager + Network Specialist |
| Patch compliance review (Tanium) | Network Specialist |
| Firewall rule review (unused/overly permissive rules) | Network Specialist |
| IAM access review (new hires, terminations, role changes) | Cybersecurity Manager |
| Proactive threat hunting (1 structured hunt/week) | Threat Analysts |
| Security awareness content/phishing simulation review | GRC Consultant (rotating) |
| Backup job success/failure verification | Network Specialist |
| Incident review & retrospective (open/closed incidents) | Cybersecurity Manager |
| PAM vault health check (stale accounts, rotation failures) | Cybersecurity Manager |

### 5.3 Monthly Activities

| Activity | Owner |
|---|---|
| Executive/board security reporting (KPIs, Section 8) | CISO |
| Compliance review (NIST 800-53/CIS control status) | GRC Consultants |
| Risk assessment updates (register maintenance) | GRC Consultants |
| Tabletop exercise (rotating scenario: ransomware, OT incident, BEC) | CISO + Cybersecurity Manager |
| DR/backup restore validation | Network Specialist |
| Third-party/vendor risk review (incl. OT vendors, SolarWinds attestations) | GRC Consultants |
| Security metrics package production | Cybersecurity Manager |
| Penetration test coordination/scoping (as scheduled) | CISO |
| Security architecture review (new projects, exceptions) | CISO + Network Specialist |

### 5.4 Quarterly Activities

| Activity | Owner |
|---|---|
| Full vulnerability assessment (internal + external) | GRC Consultants + VM platform |
| Access recertification (privileged + sensitive data roles) | GRC Consultants |
| NIST 800-53 control validation cycle | GRC Consultants |
| CIS Controls self-assessment | GRC Consultants |
| Risk register formal update & executive review | CISO + GRC Consultants |
| Red Team / Purple Team exercise (starting Q2 once baseline maturity reached) | CISO (external partner + internal analysts) |
| Business continuity testing | Cybersecurity Manager |
| OT/ICS asset inventory reconciliation | Threat Analysts (OT-trained) |

### 5.5 Annual Activities

| Activity | Owner |
|---|---|
| Enterprise penetration testing (IT + OT scope) | CISO (external partner) |
| Security program maturity review | CISO |
| Full-scale disaster recovery test | Cybersecurity Manager + Network Specialist |
| Policy review & update (all security policies) | GRC Consultants |
| Cybersecurity roadmap refresh (next 12 months) | CISO |
| External audit support (AWIA re-certification cycle, financial/compliance audits) | GRC Consultants |
| Security awareness program review & refresh | GRC Consultants |
| Technology refresh planning (contract renewals, EOL review) | CISO + Cybersecurity Manager |

---

## 6. Incident Response Plan

### 6.1 Lifecycle & Role Responsibilities

| Phase | Key Actions | Responsible Role(s) |
|---|---|---|
| **Detection** | SIEM/EDR/OT-platform alert fires or is reported (phishing, user report, MDR escalation) | MDR Provider (Tier 1) → Threat Analyst (Tier 2 confirmation) |
| **Analysis** | Scope, severity classification, affected assets identified, IT vs. OT determination made | Threat Analysts; Cybersecurity Manager for Sev-1/2 |
| **Containment** | Endpoint isolation (CrowdStrike), network segmentation enforcement (Panorama/ISE), account disablement/credential rotation (PAM), OT segment isolation if applicable | Threat Analysts + Network Specialist; CISO approves OT isolation actions |
| **Eradication** | Remove malicious artifacts, close initial access vector, patch/harden exploited weakness | Threat Analysts + Network Specialist |
| **Recovery** | Restore from validated/immutable backup, re-enable access with monitoring, confirm system integrity before full return to production (especially OT) | Network Specialist + Threat Analysts; OT vendor engaged for control-system validation |
| **Lessons Learned** | Formal after-action review within 5 business days of closure; update runbooks/risk register | CISO facilitates; GRC Consultants document; all responders contribute |

### 6.2 Incident Command Structure

- **Incident Commander**: Cybersecurity Manager (Sev-3/4); CISO (Sev-1/2, or anything touching OT/public safety/regulatory notification).
- **Technical Lead**: Senior Threat Analyst (rotating).
- **Network/Containment Lead**: Network Specialist.
- **Compliance/Notification Lead**: GRC Consultant (regulatory/AWIA/state notification obligations, breach notification timing).
- **Executive Sponsor**: CISO reports to executive leadership; CISO or delegate is spokesperson for any external communication.

### 6.3 Retained Support (Recommended Addition)

- Retained third-party **digital forensics & incident response (DFIR) firm** on standby contract — not currently evidenced. This is a **High priority** gap given the 9-person team has no dedicated IR specialist.
- Retained **ransomware negotiation/legal counsel** contact pre-identified before an incident, not during one.
- WaterISAC and CISA notification procedures documented and drilled (sector-specific reporting obligations).

### 6.4 Severity Classification (Summary)

| Severity | Definition | Response Target |
|---|---|---|
| Sev-1 | OT/SCADA compromise, public safety impact, or organization-wide outage | Immediate (15 min ack), CISO-led, executive notification within 1 hr |
| Sev-2 | Confirmed breach/ransomware in IT environment, contained to non-OT | 30 min ack, Cybersecurity Manager-led |
| Sev-3 | Isolated malware/phishing compromise, single user/endpoint | 4 hr ack |
| Sev-4 | Policy violation, low-risk anomaly | Next business day |

---

## 7. Vulnerability Management Program

### 7.1 Process

1. **Discovery** — Continuous scanning (internal + external) via the new VM platform (Item #3, Section 4), reconciled against Tanium's real-time asset inventory and the new OT asset inventory (Section 3.4). No asset — IT or OT — should exist outside at least one inventory source.
2. **Prioritization** — Risk score combines CVSS base score, EPSS (exploit prediction), asset criticality tier (OT/ICS and domain controllers = highest), and active exploitation intelligence from the TIP.
3. **Remediation** — Ticketed to system owners with SLA clock starting at confirmed detection; OT remediation requires change-window coordination with operations (patching a live treatment plant control system is never immediate).
4. **Validation** — Rescan to confirm closure; VM platform auto-closes ticket only on verified remediation, not on owner attestation alone.
5. **Reporting** — Monthly remediation-SLA compliance report to CISO; quarterly full report to executive leadership.

### 7.2 SLAs by Severity

| Severity | IT Systems | OT/ICS Systems (change-window constrained) |
|---|---|---|
| Critical (CVSS 9.0–10 or actively exploited) | 7 days | 30 days or compensating control (segmentation/monitoring) within 72 hrs |
| High (CVSS 7.0–8.9) | 30 days | 90 days |
| Medium (CVSS 4.0–6.9) | 90 days | Next scheduled maintenance window |
| Low (CVSS <4.0) | Best effort / next patch cycle | Best effort |

### 7.3 KPIs

- % of critical vulnerabilities remediated within SLA.
- Mean time to remediate (MTTR) by severity.
- Number of assets with no scan coverage (target: 0 for IT; tracked separately and driven toward 0 for OT as the program matures).
- Vulnerability recurrence rate (same CVE reappearing post-remediation, indicating patch process failure).
- Exception/risk-acceptance count and aging.

---

## 8. Security Metrics & KPIs

| Metric | Target / Direction | Reporting Cadence |
|---|---|---|
| Mean Time to Detect (MTTD) | Downward trend; target < 1 hr for critical alerts post-MDR | Monthly |
| Mean Time to Respond (MTTR) | Downward trend; target < 4 hrs containment for Sev-1/2 | Monthly |
| Open Critical Vulnerabilities (count, age) | Downward trend; 0 aged > SLA | Weekly (ops), Monthly (exec) |
| Patch Compliance % (IT/OT split) | > 95% IT within SLA; OT tracked separately given change-window constraints | Monthly |
| MFA Adoption % (privileged / all users) | 100% privileged, > 98% all users | Monthly |
| Incident Volume & Trend (by category, severity) | Trend visibility, not a raw target | Monthly |
| Endpoint Compliance % (EDR agent healthy, policy current) | > 98% | Weekly |
| Phishing Simulation Failure Rate | Downward trend; target < 5% | Quarterly (per campaign) |
| Backup Success Rate / Restore Test Success | 100% success on scheduled jobs; 100% pass on quarterly restore tests | Monthly / Quarterly |
| Privileged Account Usage (sessions, anomalous access flags) | 100% of privileged sessions recorded; anomalies investigated < 24 hrs | Weekly |
| OT/ICS Asset Inventory Completeness | Trending to 100% of known facilities | Quarterly (during rollout) |
| SOAR Playbook Automation Rate | % of Tier-1 alerts auto-triaged | Monthly (post-deployment) |

---

## 9. 12-Month Cybersecurity Roadmap

*Quick wins are front-loaded into the first 90 days per the requirement to prioritize early impact.*

### Month 1 — Foundation & Quick Wins
- **Objectives**: Establish governance rhythm; close the fastest, highest-impact gaps.
- **Projects**: Kick off MDR/24x7 coverage procurement (CrowdStrike Falcon Complete); confirm/enable backup immutability; launch security awareness platform; begin OT/ICS security assessment (vendor selection: Claroty/Dragos/Nozomi); confirm M365 E5/Entra ID P2 licensing and enforce phishing-resistant MFA for all privileged accounts.
- **Deliverables**: Signed MDR contract; immutable backup confirmed for Tier-1 systems; awareness platform live; OT vendor shortlist.
- **Resources**: CISO, Cybersecurity Manager, Network Specialist.
- **Outcomes**: Immediate reduction in ransomware blast-radius risk and after-hours detection gap started.

### Month 2 — Identity & Email Hardening
- **Objectives**: Close initial-access vector gaps.
- **Projects**: Deploy advanced email security (Proofpoint/Mimecast/Abnormal) as pilot; begin DLP policy design (Purview); begin secrets management pilot.
- **Deliverables**: Email security pilot live for highest-risk mailboxes (executives, finance, ops); DLP policy draft.
- **Resources**: Network Specialist, Cybersecurity Manager, GRC Consultants (policy input).
- **Outcomes**: Reduced phishing/BEC success likelihood.

### Month 3 — Vulnerability Management Stand-Up
- **Objectives**: Replace ad hoc patch visibility with a real VM program.
- **Projects**: Deploy VM platform (Tenable/Rapid7); baseline scan of full IT estate; finalize SLA policy (Section 7); MDR coverage goes fully live.
- **Deliverables**: First full vulnerability baseline report; SLA policy signed by leadership.
- **Resources**: Cybersecurity Manager, Network Specialist, GRC Consultants.
- **Outcomes**: End of Q1 — 24x7 detection live, email hardened, VM program operating. **90-day quick-win milestone met.**

### Month 4 — SOAR & Threat Intel
- **Objectives**: Multiply analyst capacity.
- **Projects**: Deploy SOAR integrated with QRadar; build first 3 automation playbooks (phishing triage, EDR isolation, IOC blocklist push); integrate WaterISAC + commercial TIP feed.
- **Deliverables**: SOAR live with 3 playbooks in production.
- **Resources**: Threat Analysts, Cybersecurity Manager.
- **Outcomes**: Reduced Tier-1 manual workload; faster containment.

### Month 5 — CASB/SSPM & Attack Surface Management
- **Objectives**: Close SaaS and external-exposure blind spots.
- **Projects**: Deploy CASB/SSPM; deploy ASM tool; complete DLP rollout beyond pilot.
- **Deliverables**: SaaS posture baseline report; external attack surface baseline.
- **Resources**: Network Specialist, Cybersecurity Manager.
- **Outcomes**: Shadow SaaS and internet-facing exposure now visible and tracked.

### Month 6 — IR Formalization & First Tabletop
- **Objectives**: Operationalize incident response.
- **Projects**: Finalize written IR plan and runbooks (Section 6); contract retained DFIR firm; run first tabletop (ransomware scenario).
- **Deliverables**: Signed IR plan; DFIR retainer contract; tabletop after-action report.
- **Resources**: CISO, Cybersecurity Manager, all Threat Analysts.
- **Outcomes**: End of Month 6 — detection, response, and vulnerability management all materially matured; identity and data protection gaps closed.

### Month 7 — OT/ICS Deployment (Full Rollout)
- **Objectives**: Deliver on the top strategic priority.
- **Projects**: Deploy OT monitoring sensors across all treatment/pumping/distribution sites; establish OT asset inventory; integrate OT alerts into QRadar.
- **Deliverables**: OT platform live at first wave of sites; OT asset inventory v1.
- **Resources**: Network Specialist, Threat Analysts (OT-trained), external OT integration partner.
- **Outcomes**: First real-time OT visibility in MWD's history.

### Month 8 — IT/OT Segmentation Project
- **Objectives**: Enforce the IT/OT boundary architecturally, not just detect across it.
- **Projects**: Panorama/ISE segmentation project at the Purdue Model boundary; remove any direct OT-to-internet paths; vendor remote-access brokered through PAM only.
- **Deliverables**: Segmentation architecture implemented at first wave of sites.
- **Resources**: Network Specialist, CISO, OT integration partner.
- **Outcomes**: Materially reduced IT-to-OT lateral movement risk.

### Month 9 — CNAPP/CSPM & PKI
- **Objectives**: Close remaining cloud and certificate-hygiene gaps.
- **Projects**: Deploy CNAPP/CSPM; deploy PKI/certificate lifecycle management; run Purple Team exercise.
- **Deliverables**: Cloud posture baseline; certificate inventory with expiry alerting live.
- **Resources**: Network Specialist, Cybersecurity Manager, external Purple Team partner.
- **Outcomes**: End of Q3 — full architecture from Section 3 substantially deployed.

### Month 10 — Control Validation & Access Recertification
- **Objectives**: Prove the program against its frameworks.
- **Projects**: Formal NIST 800-53 control validation cycle; CIS Controls self-assessment; quarterly access recertification.
- **Deliverables**: Control validation report with residual-gap list; recertification sign-off.
- **Resources**: GRC Consultants.
- **Outcomes**: Documented compliance posture, ready for external audit.

### Month 11 — External Testing
- **Objectives**: Independent validation of the year's work.
- **Projects**: External penetration test scoped to IT **and** OT; DR/BC full-scale test including immutable backup restore.
- **Deliverables**: Pen test report with remediation tracking; DR test after-action report.
- **Resources**: CISO, Cybersecurity Manager, Network Specialist, external pen-test firm.
- **Outcomes**: Independent confirmation of risk reduction achieved.

### Month 12 — Program Review & Next-Year Roadmap
- **Objectives**: Close the loop; plan Year 2.
- **Projects**: Full security program maturity review; policy refresh; AWIA re-certification readiness check; board/executive annual report; technology refresh planning; Year 2 roadmap drafted.
- **Deliverables**: Annual report to executive leadership/board; Year 2 roadmap.
- **Resources**: CISO, full team.
- **Outcomes**: Measurable maturity improvement from ~2.6/5 baseline (Section 1.4) into the "Managed" range, with OT visibility, 24x7 coverage, and a real VM program now permanent fixtures rather than gaps.

---

## 10. Project Plan

The full task-level project plan (phases, milestones, durations, predecessors, resource assignments, deliverables, and success criteria) is provided as a companion file formatted for direct import into Microsoft Project or Excel:

**`Project-Plan.csv`** (in this repository)

Import notes for Microsoft Project: File → Open → select CSV → map columns (Task Name, Duration, Predecessors, Resource Names) → set the project **Start Date** to the actual kickoff date; MS Project will auto-schedule all successor tasks from the Predecessors column. In Excel, the file opens directly as a formatted task table and can be used as-is for tracking or converted to a Gantt view with the built-in "Conditional Formatting → Data Bars" against the Duration column.

---

## 11. NIST SP 800-53 Rev. 5 Control Mapping

Mapping of proposed technologies/processes to control families. **Status** reflects the posture *after* this roadmap's Year-1 execution, with the pre-roadmap gap noted where relevant.

| Family | Family Name | Key Controls Addressed | Addressing Technology/Process | Status After Year 1 |
|---|---|---|---|---|
| **AC** | Access Control | AC-2 (Account Mgmt), AC-3, AC-6 (Least Privilege), AC-17 (Remote Access) | Centrify/Delinea PAM (enhanced), Zscaler ZPA, Entra ID Conditional Access, quarterly recertification | Fully addressed (was: Partial — no JIT/recertification) |
| **AU** | Audit & Accountability | AU-2, AU-6 (Audit Review), AU-12 (Generation) | QRadar SIEM + SOAR + expanded log sources (OT, CASB, DLP, email) | Fully addressed (was: Partial — 24x7 review gap) |
| **CA** | Assessment, Authorization & Monitoring | CA-2 (Assessments), CA-7 (Continuous Monitoring) | GRC Consultants' quarterly control validation; CSPM/VM platforms feeding continuous monitoring | Fully addressed (was: Partial — point-in-time only) |
| **CM** | Configuration Management | CM-2, CM-6 (Config Settings), CM-8 (Component Inventory) | Tanium (IT) + new OT asset inventory (Claroty/Dragos/Nozomi) | Fully addressed for IT; **substantially improved** for OT (was: Not addressed for OT) |
| **CP** | Contingency Planning | CP-9 (Backup), CP-10 (Recovery) | Immutable backup (3-2-1-1), quarterly restore tests, annual full DR test | Fully addressed (was: Partial — immutability/testing unconfirmed) |
| **IA** | Identification & Authentication | IA-2 (MFA), IA-5 (Authenticator Mgmt) | Phishing-resistant MFA via Entra ID; PAM credential rotation | Fully addressed (was: Partial) |
| **IR** | Incident Response | IR-4 (Handling), IR-6 (Reporting), IR-8 (Plan) | Formal IR plan (Section 6), SOAR playbooks, retained DFIR firm | Fully addressed (was: Partial — informal only) |
| **MA** | Maintenance | MA-2, MA-4 (Remote Maintenance) | PAM-brokered vendor/OT remote maintenance access (no standing VPN) | Fully addressed (was: Not addressed for OT vendor access) |
| **MP** | Media Protection | MP-5 (Media Transport), MP-7 (Media Use) | DLP policies extended to removable media/egress channels | Substantially improved (was: Not addressed) |
| **PE** | Physical & Environmental Protection | PE-3, PE-6 | Out of scope for this cyber-focused roadmap — recommend confirming existing physical security program at treatment/pumping facilities aligns with OT criticality | Not addressed by this roadmap (flag for separate physical security review) |
| **PL** | Planning | PL-2 (System Security Plans), PL-8 (Architecture) | Section 3 architecture; annual policy review | Fully addressed (was: Partial) |
| **PS** | Personnel Security | PS-3 (Screening), PS-4 (Termination) | IAM access review tied to HR termination feed (weekly cadence, Section 5.2) | Substantially improved (was: Partial — manual/ad hoc) |
| **RA** | Risk Assessment | RA-3 (Risk Assessment), RA-5 (Vulnerability Monitoring) | Dedicated VM platform + risk register (GRC-owned) | Fully addressed (was: Partial — no continuous scanning) |
| **SA** | System & Services Acquisition | SA-9 (External Systems), SA-22 (Unsupported Components) | OT vendor risk review (Section 5.4), SolarWinds supply-chain governance | Substantially improved (was: Not formally addressed) |
| **SC** | System & Communications Protection | SC-7 (Boundary Protection), SC-8 (Transmission Confidentiality) | Panorama/ISE IT/OT segmentation, PKI/cert lifecycle mgmt | Fully addressed for IT; **substantially improved** for OT boundary (was: Not addressed for OT) |
| **SI** | System & Information Integrity | SI-3 (Malicious Code), SI-4 (Monitoring), SI-7 (Integrity) | CrowdStrike + OT monitoring platform + FIM on critical servers | Fully addressed (was: Partial — no OT/FIM coverage) |

**Not fully addressed after Year 1 (carry into Year 2 roadmap):** PE (physical security review recommended as a separate, facilities-partnered workstream); SR (Supply Chain Risk Management) formalization beyond the SolarWinds/OT-vendor reviews started in Year 1; PM (Program Management) family formal documentation (security program charter, resource plan) as a Year 2 documentation exercise once operational gaps are closed.

---

## Appendix: Staffing Recommendation Summary

The technology roadmap above assumes the **existing 9-person team plus the managed-service augmentations** (MDR, retained DFIR) called out in Sections 4 and 6 — not a large internal hiring wave. If MWD prefers to insource rather than use managed services, the minimum incremental headcount to run everything in this roadmap without MDR augmentation would be approximately:

- 2–3 additional Threat Analysts (to reach 24x7 coverage internally)
- 1 dedicated OT/ICS Security Engineer
- 1 dedicated Vulnerability Management Analyst
- 1 Security Architect

The MDR/managed-services path in this roadmap is recommended as the faster, lower-risk, and typically lower-total-cost option for closing the 24x7 and OT gaps within the 12-month window, with insourcing revisited in the Year 2 roadmap refresh (Month 12) once budget and hiring lead time allow.
