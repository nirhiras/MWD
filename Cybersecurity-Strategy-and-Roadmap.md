# Enterprise Cybersecurity Strategy & Roadmap
### Prepared for: Water District (Water Utility / Critical Infrastructure — Water & Wastewater Systems Sector)
**Prepared by:** Office of the CISO (Strategy Document)
**Date:** July 2026
**Classification:** Internal Use Only — Security Sensitive

---

## How to Read This Document

This strategy treats Water District as a **critical infrastructure operator** first and an 1,800-person enterprise second. That framing drives several recommendations that a generic IT security assessment would miss: OT/ICS (SCADA) protection for treatment and distribution systems, alignment with the **America's Water Infrastructure Act (AWIA) §2013** risk and resilience assessment cycle, CISA's **Water and Wastewater Systems Sector** guidance, WaterISAC threat sharing, and NIST SP 800-82 (ICS security) alongside the requested NIST SP 800-53 Rev. 5 and CIS Controls v8 mappings.

The existing team — CISO, Cybersecurity Manager, a 24x7 Security Operations Center (3 shifts, minimum 2–3 SOC Analysts per shift), 1 Network Specialist, and 3 GRC Consultants (roughly 14 FTE total; planning assumption of ~8 SOC Analysts across shifts) — is better operationally staffed than most water utilities this size. Round-the-clock detection coverage is already in place, OT/ICS runs on its own dedicated infrastructure with its own monitoring tooling, an Incident Response playbook is already written and ready to use, and PKI/certificate lifecycle management is already operationalized via Yubico. The gaps that remain are narrower and more specific than a generic "build everything from zero" assessment would suggest: no dedicated vulnerability management analyst, no dedicated IAM/PAM administrator, no security architecture function, and no confirmed integration between the OT monitoring tooling and the central SIEM/SOC workflow. This report is oriented accordingly — toward closing those specific gaps and strengthening what already exists (formalizing the IR playbook's review cadence, automating the well-staffed SOC with SOAR) rather than standing up core capabilities that are already there.

---

## 1. Executive Summary

### 1.1 Current Security Posture

Water District has invested in a credible foundation of enterprise-grade tooling: CrowdStrike Falcon (EDR), IBM QRadar (SIEM), Palo Alto Panorama (NGFW management), Cisco ISE (NAC), Cisco Umbrella (DNS security), Zscaler ZPA/VPN (remote access), Tanium (endpoint visibility/management), Centrify/Delinea (PAM), VMware Horizon (VDI), Microsoft 365, and SolarWinds (network monitoring) — backed by genuinely solid operational staffing (a 24x7 SOC), separate OT/ICS infrastructure with its own monitoring, an existing IR playbook, and Yubico-based PKI. This is a mid-to-upper maturity **tooling and staffing** posture overall. The remaining maturity gap is narrower: specialized ownership (vulnerability management, IAM/PAM, security architecture) and cross-domain integration/automation (OT-to-SIEM, SOAR) rather than foundational process or coverage.

### 1.2 Strengths

- **24x7 Security Operations Center** already staffed across 3 shifts (minimum 2–3 SOC Analysts per shift) — genuinely ahead of most peer utilities, which frequently lack round-the-clock coverage entirely.
- **OT/ICS on separate, dedicated infrastructure with its own monitoring tooling** — the OT network is architecturally isolated and already instrumented, a meaningful head start most water utilities have not reached.
- **Incident Response playbook already exists and is ready to use** — the work ahead is governance (a recurring review cadence) and augmentation (retained forensics support), not authoring a plan from scratch.
- **PKI and certificate lifecycle management already operationalized via Yubico**, reducing cert-expiry outage risk and supporting hardware-backed, phishing-resistant authentication.
- **Strong endpoint and network security tooling**: CrowdStrike + Tanium give strong endpoint telemetry and control; Panorama + ISE + Umbrella give layered network/DNS control.
- **Centralized SIEM** (QRadar) already exists as an aggregation point for correlation and compliance evidence.
- **PAM foundation** in place (Centrify/Delinea) rather than being built from zero.
- **Dedicated GRC capacity** (3 consultants) — a rarity at this org size — gives Water District a real head start on continuous NIST 800-53 / CIS control assessment rather than annual scramble audits.
- **Zero Trust remote access foundation** via Zscaler ZPA reduces flat-VPN exposure already.

### 1.3 Key Risks

| Risk | Why It Matters |
|---|---|
| **OT monitoring is not confirmed to be integrated with the central SIEM/SOC** | OT/ICS already runs on separate infrastructure with its own monitoring tooling, but if those alerts don't reach QRadar and the 24x7 SOC's triage workflow, OT anomalies may go unactioned outside the OT team's own visibility — a correlation and coverage-continuity gap rather than a detection gap. |
| **IR playbook lacks a confirmed, formal review cadence** | A ready-to-use IR playbook exists, but without a scheduled annual/quarterly review it drifts out of date (stale contacts, unlisted new platforms, no lessons-learned loop from real incidents or tabletops). |
| **No enterprise vulnerability management platform** | Tanium provides asset/patch visibility but is not a substitute for a dedicated, risk-scored VM program (CVSS + EPSS + asset criticality). |
| **No dedicated email security layer beyond M365 native** | M365 EOP/Defender for Office (base) is insufficient against BEC and credential-phishing given water-utility targeting by ransomware affiliates. |
| **No CASB/SSPM or CNAPP/CSPM** | Cloud/SaaS posture (M365 and any IaaS) is effectively unmonitored for misconfiguration and shadow SaaS. |
| **SolarWinds as network monitoring platform** | Post-SUNBURST (2020), SolarWinds requires explicit supply-chain risk management (patch attestation, network segmentation of the management plane, no direct internet egress) — not a reason to replace it, but a reason to formally govern it. |
| **No formal secrets management tooling** | Increases risk of hardcoded credentials/API keys in applications and service accounts (PKI/certificate lifecycle is already covered separately via Yubico). |
| **Specialized ownership gaps despite strong SOC staffing** | The 24x7 SOC itself is well staffed, but vulnerability management, IAM/PAM administration, and security architecture have no dedicated owner — they're absorbed implicitly by the Cybersecurity Manager and Network Specialist alongside their other duties. |

### 1.4 Security Maturity (Composite, NIST CSF-style scoring, 1–5)

| Function | Maturity | Notes |
|---|---|---|
| Identify | 3.0 | GRC capacity is strong; OT asset inventory exists but reconciliation with the central CMDB/Tanium is unconfirmed |
| Protect | 3.3 | Strong endpoint/network controls and PKI (Yubico); gaps remain in DLP, email, secrets management |
| Detect | 3.4 | 24x7 SOC and separate OT monitoring tooling both already in place; the gap is cross-domain correlation (OT-to-SIEM) and automation, not coverage hours |
| Respond | 3.0 | IR playbook already exists and the SOC runs 24x7; still missing a formal review cadence, a retained DFIR firm, and SOAR automation |
| Recover | 2.3 | Backup exists (implied via SolarWinds/infra) but immutability/testing not confirmed |
| **Overall** | **~3.0 / 5 (Defined → Managed)** | Tooling and core operational staffing are both genuinely solid; remaining gaps are specialized ownership (VM, IAM/PAM, architecture) and integration/automation (OT-SIEM, SOAR), not foundational capability |

### 1.5 Strategic Priorities (Next 12 Months)

1. **Integrate existing OT monitoring tooling with the central SIEM/SOC** so OT alerts get the same 24x7 triage discipline as IT alerts — the highest-value remaining sector-specific gap, since OT visibility itself already exists.
2. **Formalize the IR playbook's review cadence** (annual full review, quarterly check-in) and add a retained DFIR firm for surge forensics support.
3. **Stand up a real vulnerability management program** with SLA-driven remediation, not just patch visibility.
4. **Close remaining identity/data-protection gaps**: PAM depth (session recording, JIT), secrets management, DLP, email security, CASB/CNAPP.
5. **Deploy SOAR** to multiply the throughput of the already-staffed 24x7 SOC through automation.
6. **Formalize backup immutability and ransomware recovery testing** — assume ransomware is the primary scenario to defend against.

### 1.6 Top 5 Recommendations

1. Integrate the existing OT monitoring platform's alerts into QRadar so the 24x7 SOC gains OT visibility without a second console — the fastest, lowest-cost way to close the remaining OT gap since the underlying tooling is already owned.
2. Deploy a **dedicated vulnerability management platform** (Tenable or Rapid7) with SLA-based remediation tracked as a KPI.
3. Deploy **SOAR** integrated with QRadar to multiply the effective capacity of the existing 24x7 SOC through automation.
4. Formalize the existing **IR playbook's review cadence** (annual + quarterly) and add a retained DFIR firm for surge forensics support.
5. Formalize **backup immutability and ransomware recovery testing** (air-gapped/immutable copies, quarterly restore tests).

---

## 2. Gap Assessment

### 2.1 Technology Stack Coverage Matrix

| Security Capability | Current State | Gap |
|---|---|---|
| Endpoint Protection (EDR) | CrowdStrike Falcon | Adequate — already paired with an internal 24x7 SOC; no managed-response augmentation needed |
| SIEM | IBM QRadar | Adequate platform; the 24x7 SOC already provides coverage hours — the remaining gap is content tuning capacity and SOAR-driven automation, not staffing hours |
| Network Firewall Mgmt | Palo Alto Panorama | Adequate — formal rule review cadence needed |
| NAC | Cisco ISE | Adequate — validate/extend full 802.1X + OT network segmentation enforcement given OT's separate infrastructure |
| DNS/Web Security | Cisco Umbrella | Adequate |
| Remote Access / ZTNA | Zscaler ZPA/VPN | Adequate — retire remaining legacy VPN in favor of full ZTNA |
| Endpoint/Asset Mgmt | Tanium | Strong for IT; OT/ICS asset inventory exists separately via the OT monitoring tooling — reconciliation into a single asset register is unconfirmed |
| PAM | Centrify/Delinea | Foundation present — depth gap (session recording, JIT/JEA, secrets vaulting) |
| VDI | VMware Horizon | Adequate |
| Productivity/Email | Microsoft 365 | **Native email security insufficient**; Entra ID licensing tier needs confirmation for Conditional Access/Identity Protection |
| Network Monitoring | SolarWinds | Adequate for NPM; **supply-chain governance gap** (patch attestation, segmentation) |
| **OT/ICS Security** | Separate OT infrastructure with its own dedicated monitoring tooling already in place | Medium gap — integration with the central SIEM/SOC for unified correlation and alerting is unconfirmed |
| **SOAR** | None | High gap — analyst capacity multiplier missing |
| **Threat Intelligence Platform** | Ad hoc | Medium gap — no structured TIP or feed curation beyond vendor defaults |
| **Vulnerability Management** | Tanium (partial) | High gap — no dedicated, risk-prioritized VM platform |
| **Email Security (advanced)** | M365 native only | High gap — BEC/phishing is the #1 initial access vector |
| **CASB/SSPM** | None | Medium-high gap — no visibility into SaaS posture/shadow IT |
| **CNAPP/CSPM** | None | Medium-high gap (scales with any cloud/IaaS footprint) |
| **DLP** | None dedicated (M365 Purview unconfirmed) | High gap for a utility handling PII, OT design data, SCADA network diagrams |
| **Secrets Management** | None | Medium-high gap |
| **PKI / Certificate Lifecycle** | Yubico-based PKI and certificate lifecycle management in place | Low gap — confirm scope covers all server/service certificates (not just user/device authentication) and integrate expiry alerting into the SIEM |
| **Attack Surface Management** | None | Medium gap — no continuous external attack surface visibility |
| **Security Awareness Training** | Unconfirmed/ad hoc | Medium gap |
| **Backup Immutability/Ransomware Recovery** | Unconfirmed | High gap given ransomware targeting of utilities |
| **Asset Discovery (OT)** | Covered by existing OT monitoring tooling | Low gap — reconcile with the central asset register (Tanium/CMDB) for a single source of truth |

### 2.2 Operational Gaps

- OT monitoring tooling operates separately from the core cyber team; confirm a named liaison/ownership model and SLA between OT operations and the SOC, and integrate OT alerts into the central SIEM/SOC workflow.
- No confirmed, formal recurring (annual + quarterly) review cadence for the existing IR playbook.
- No formal IR retainer with outside forensics/negotiation counsel.
- No SOAR — response time is bound by manual analyst throughput despite strong SOC staffing.
- No dedicated vulnerability management analyst role — VM is implicitly split across SOC Analysts/Network Specialist.
- No IAM/PAM administrator role distinct from the Network Specialist/Cybersecurity Manager.
- No security architecture function (architecture decisions currently default to whoever is available).

### 2.3 Compliance Gaps (Preview — full mapping in Section 11)

- **AC (Access Control)**: Partially met — PAM exists but least-privilege/JIT and periodic access recertification are not evidenced as formal, recurring processes.
- **CP (Contingency Planning)**: Partially met — backup exists but immutability and full-scale DR testing are unconfirmed.
- **IR (Incident Response)**: Substantially met — a ready-to-use IR playbook already exists; recommend confirming and formalizing an annual/quarterly review cadence (Section 6 fixes this).
- **RA (Risk Assessment)**: Partially met — 3 GRC consultants perform assessments, but no continuous vulnerability scanning/risk-scoring feeds this.
- **SC (System & Communications Protection)**: Substantially met — OT already runs on separate infrastructure; confirm the IT/OT boundary enforcement and monitoring integration are formally documented and tested.
- **SI (System & Information Integrity)**: Partially met — no dedicated FIM; OT anomaly detection exists via the separate OT tooling but SIEM integration is unconfirmed.
- **SR (Supply Chain Risk Management)**: Not formally addressed (SolarWinds governance, OT vendor/integrator risk).

### 2.4 High-Risk Areas (Ranked)

1. Ransomware resilience (backup immutability + tested recovery).
2. OT monitoring integration with the central SIEM/SOC (visibility already exists; correlation and unified triage do not).
3. Vulnerability remediation velocity (no SLA-driven program).
4. Initial-access vectors: phishing/BEC (email security gap) and unmanaged SaaS (CASB/SSPM gap).
5. Third-party/OT vendor and SolarWinds supply-chain exposure.

---

## 3. Recommended Security Architecture

Design principles: **Zero Trust**, **Defense in Depth**, **Least Privilege**, **Identity-First Security**, **Continuous Monitoring** — extended to explicitly cover the **IT/OT boundary**, which is the architectural feature most enterprise security architectures omit and which matters most for Water District.

### 3.1 Identity
- Entra ID (M365) as the identity backbone; confirm **P2 licensing** for Identity Protection + risk-based Conditional Access.
- Phishing-resistant MFA (FIDO2/WebAuthn) for all privileged and remote-access accounts; app-based MFA minimum for standard users.
- Centrify/Delinea PAM extended to: session recording for all privileged sessions, just-in-time (JIT) elevation, vaulting of all shared/service accounts, and rotation automation.
- Conditional Access policies enforced at Zscaler ZPA and M365 boundary (device compliance + user risk + location).
- Quarterly access recertification for privileged and sensitive-data-access roles (owned by GRC consultants, executed via IAM tooling — see Section 5).

### 3.2 Endpoints (IT)
- CrowdStrike Falcon retained as EDR of record; the existing internal 24x7 SOC (3 shifts, 2–3 SOC Analysts per shift) performs Tier-1/2 monitoring, response, and proactive threat hunting — no managed-detection augmentation needed for coverage hours.
- Tanium retained for real-time asset inventory, patch enforcement, and configuration compliance (CIS benchmarks).
- Application allowlisting evaluated for high-risk endpoints (HMI workstations, engineering laptops) — see OT section.

### 3.3 Servers
- CrowdStrike Falcon extended to all server workloads (Windows/Linux, physical and virtual).
- Tanium for patch/configuration compliance against CIS Benchmarks.
- File Integrity Monitoring (FIM) enabled on servers handling regulated data and control-system historian/engineering servers.
- Server tiering (Tier 0/1/2 administrative model) enforced through PAM.

### 3.4 OT/ICS (SCADA) — Integration & Assurance Domain
- Water District already operates OT/ICS (SCADA) on **separate, dedicated infrastructure with its own OT-specific monitoring tooling** — the architectural isolation and base monitoring capability this roadmap would otherwise need to build from scratch already exist.
- **Feed the existing OT monitoring platform's alerts into QRadar** via its native SIEM connector so the 24x7 SOC gains OT visibility and triage responsibility without standing up a second console or a separate OT-only watch function.
- **Validate and harden the IT/OT network segmentation boundary** using existing Palo Alto Panorama + Cisco ISE (confirm unidirectional gateways or tightly ruled firewalls at the Purdue Model Level 3/4 boundary are in place and tested); confirm no direct OT-to-internet egress exists.
- **Reconcile the existing OT asset inventory** with Tanium/the central asset register so IT and OT assets are visible from one source of truth.
- Establish a named liaison/ownership model and SLA between the team operating the OT monitoring tooling and the central SOC for alert handoff and escalation.
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
- PKI/certificate lifecycle management is already operationalized via **Yubico**; confirm its scope covers all server/service certificates in addition to user/device authentication, and integrate certificate expiry alerting into the SIEM to eliminate expiry-driven outages.

### 3.10 Logging
- QRadar remains the SIEM of record; expand log source coverage to include OT platform alerts, CASB/CNAPP findings, DLP events, and email security verdicts.
- Centralize retention policy aligned to NIST AU family and any state/AWIA record-retention requirements.

### 3.11 Security Operations
- SOC model: the existing 24x7 SOC (3 shifts, minimum 2–3 SOC Analysts per shift) already provides continuous Tier-1/2 monitoring, response, and hunting — no external managed-detection provider is needed to close a coverage-hours gap.
- SOAR deployed to automate Tier-1 triage/enrichment playbooks (phishing report handling, EDR isolation, IOC blocklisting), directly multiplying each shift's effective throughput.
- A rotating OT-liaison SOC Analyst ensures OT alerts (once integrated into QRadar per Section 3.4) receive the same triage discipline as IT alerts, on every shift.
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
| 1 | OT-to-SIEM Integration (Connector/Middleware) | Vendor-specific connector for the existing OT monitoring platform | Feed already-deployed OT monitoring alerts into QRadar for unified correlation and SOC triage | OT monitoring tooling exists but integration with the central SIEM/SOC is unconfirmed | **Medium** | One-time integration effort + minor incremental connector license | Phase 3 (Month 7) |
| 2 | Off-Hours Surge/Overflow Support (optional) | CrowdStrike Falcon Complete/OverWatch (already-licensed EDR vendor) | Optional surge capacity for simultaneous/major incidents on top of the existing internal 24x7 SOC | 24x7 coverage is already staffed internally; this is insurance against a major-incident spike, not a coverage gap | **Low (optional)** | Per-endpoint annual subscription | Not scheduled — evaluate only if a major incident exposes surge-capacity strain |
| 3 | Vulnerability Management Platform | Tenable.io / Tenable.sc, Rapid7 InsightVM | Risk-prioritized, SLA-driven vulnerability management | No dedicated VM tool today; Tanium ≠ full VM program | **Critical** | Per-asset annual subscription | Phase 1 (Months 2–3) |
| 4 | SOAR | Palo Alto Cortex XSOAR, IBM QRadar SOAR | Automate Tier-1 triage/response playbooks | Multiplies analyst capacity; enables consistent IR execution | **High** | Per-analyst/user annual subscription | Phase 2 (Months 4–5) |
| 5 | Advanced Email Security | Proofpoint, Mimecast, Abnormal Security | Anti-phishing/BEC/impersonation protection layered on M365 | #1 initial-access vector not adequately covered natively | **High** | Per-mailbox annual subscription | Phase 1 (Months 2–3) |
| 6 | DLP | Microsoft Purview DLP (leverages existing M365 E5 if licensed) | Prevent exfiltration of PII/OT design data | No enterprise DLP today | **High** | Included in M365 E5 or add-on per-user | Phase 1 (Months 2–3) |
| 7 | CASB / SSPM | Microsoft Defender for Cloud Apps, Netskope | SaaS posture monitoring, shadow IT discovery | No SaaS security visibility today | **High** | Per-user annual subscription | Phase 2 (Months 4–5) |
| 8 | CNAPP / CSPM | Microsoft Defender for Cloud, Wiz, Prisma Cloud | Cloud misconfiguration & workload protection | No cloud posture tool today | **Medium-High** | Per-workload/resource annual subscription | Phase 3 (Months 7–8) |
| 9 | Threat Intelligence Platform | Recorded Future, Anomali, WaterISAC feed integration | Structured, sector-relevant threat intel curation | Currently ad hoc; sector-specific intel (WaterISAC) underused | **Medium-High** | Per-analyst annual subscription | Phase 2 (Months 4–5) |
| 10 | Attack Surface Management | CyCognito, IBM Randori, Censys | Continuous external attack surface discovery | No outside-in visibility today | **Medium** | Flat annual subscription | Phase 2 (Months 5–6) |
| 11 | Secrets Management | HashiCorp Vault, CyberArk Conjur | Eliminate hardcoded credentials/API keys | No secrets vaulting today; complements Centrify/Delinea PAM | **Medium-High** | Per-node/secret annual subscription | Phase 1 (Months 3–4) |
| 12 | Certificate Program Assurance (no new product) | N/A — confirm scope of existing Yubico PKI | Confirm certificate lifecycle coverage extends to all server/service certificates, not just user/device authentication; integrate expiry alerting into the SIEM | PKI already exists via Yubico; remaining work is scope confirmation and monitoring integration, not a new deployment | **Low** | N/A — existing investment | Phase 3 (Month 9) |
| 13 | Security Awareness Training Platform | KnowBe4, Proofpoint Security Awareness | Phishing simulation + training | Ad hoc/unconfirmed program today | **High** | Per-user annual subscription | Phase 1 (Month 1) |
| 14 | Immutable Backup / Ransomware Recovery | Veeam (with immutable/object-lock repository), Rubrik | Guaranteed clean recovery copy | Backup immutability unconfirmed; ransomware is top scenario | **Critical** | Per-TB/capacity annual subscription | Phase 1 (Months 1–2) |

---

## 5. Security Operations Plan

### 5.1 Daily Activities

| Activity | Owner |
|---|---|
| SIEM alert monitoring & triage (24x7, 3-shift SOC rotation) | SOC Analysts (all shifts) |
| Endpoint (CrowdStrike) alert review & isolation actions | SOC Analysts |
| Incident triage & initial classification | SOC Analysts (shift on duty) |
| Threat intelligence feed review (WaterISAC + commercial TIP) | SOC Analysts |
| PAM session/vault activity review (privileged logins, anomalies) | Cybersecurity Manager |
| Firewall/Panorama policy hit and deny-log review | Network Specialist |
| OT/ICS anomaly alert review (via existing OT monitoring platform) | SOC Analysts (rotating OT-liaison analyst, once integrated into QRadar) |
| DNS/Umbrella blocked-request review | Network Specialist |
| Email security quarantine/BEC alert review | SOC Analysts |
| Vulnerability scanner exception/false-positive triage | Cybersecurity Manager (until dedicated VM analyst hired) |

### 5.2 Weekly Activities

| Activity | Owner |
|---|---|
| Vulnerability scan review & remediation tracking | Cybersecurity Manager + Network Specialist |
| Patch compliance review (Tanium) | Network Specialist |
| Firewall rule review (unused/overly permissive rules) | Network Specialist |
| IAM access review (new hires, terminations, role changes) | Cybersecurity Manager |
| Proactive threat hunting (1 structured hunt/week) | SOC Analysts |
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
| OT/ICS asset inventory reconciliation | SOC Analysts (OT-trained) |
| IR playbook quarterly review and update (contacts, procedures, lessons-learned) | CISO + Cybersecurity Manager |

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
| Full annual IR playbook review and formal reissue | CISO |

---

## 6. Incident Response Plan

Water District already maintains a documented, ready-to-use Incident Response playbook covering the lifecycle below. The work in this roadmap is governance (a confirmed, recurring review cadence — Section 6.5) and augmentation (retained DFIR support — Section 6.3), not authoring a plan from scratch.

### 6.1 Lifecycle & Role Responsibilities

| Phase | Key Actions | Responsible Role(s) |
|---|---|---|
| **Detection** | SIEM/EDR/OT-platform alert fires or is reported (phishing, user report, SOC escalation) | SOC Analysts (shift on duty) → shift lead/Cybersecurity Manager (Tier 2 confirmation) |
| **Analysis** | Scope, severity classification, affected assets identified, IT vs. OT determination made | SOC Analysts; Cybersecurity Manager for Sev-1/2 |
| **Containment** | Endpoint isolation (CrowdStrike), network segmentation enforcement (Panorama/ISE), account disablement/credential rotation (PAM), OT segment isolation if applicable | SOC Analysts + Network Specialist; CISO approves OT isolation actions |
| **Eradication** | Remove malicious artifacts, close initial access vector, patch/harden exploited weakness | SOC Analysts + Network Specialist |
| **Recovery** | Restore from validated/immutable backup, re-enable access with monitoring, confirm system integrity before full return to production (especially OT) | Network Specialist + SOC Analysts; OT vendor engaged for control-system validation |
| **Lessons Learned** | Formal after-action review within 5 business days of closure; update runbooks/risk register | CISO facilitates; GRC Consultants document; all responders contribute |

### 6.2 Incident Command Structure

- **Incident Commander**: Cybersecurity Manager (Sev-3/4); CISO (Sev-1/2, or anything touching OT/public safety/regulatory notification).
- **Technical Lead**: Senior SOC Analyst / shift lead (rotating).
- **Network/Containment Lead**: Network Specialist.
- **Compliance/Notification Lead**: GRC Consultant (regulatory/AWIA/state notification obligations, breach notification timing).
- **Executive Sponsor**: CISO reports to executive leadership; CISO or delegate is spokesperson for any external communication.

### 6.3 Retained Support (Recommended Addition)

- Retained third-party **digital forensics & incident response (DFIR) firm** on standby contract — not currently evidenced. Even with a well-staffed 24x7 SOC and an existing playbook, deep forensic investigation and legal-defensible evidence handling for a major incident (e.g., ransomware, OT compromise) typically exceed what an internal team should do alone — this remains a **High priority** gap.
- Retained **ransomware negotiation/legal counsel** contact pre-identified before an incident, not during one.
- WaterISAC and CISA notification procedures documented and drilled (sector-specific reporting obligations).

### 6.4 Severity Classification (Summary)

| Severity | Definition | Response Target |
|---|---|---|
| Sev-1 | OT/SCADA compromise, public safety impact, or organization-wide outage | Immediate (15 min ack), CISO-led, executive notification within 1 hr |
| Sev-2 | Confirmed breach/ransomware in IT environment, contained to non-OT | 30 min ack, Cybersecurity Manager-led |
| Sev-3 | Isolated malware/phishing compromise, single user/endpoint | 4 hr ack |
| Sev-4 | Policy violation, low-risk anomaly | Next business day |

### 6.5 IR Playbook Maintenance Cadence

The existing IR playbook is not static: it is reviewed and updated on a confirmed recurring schedule rather than left until the next real incident forces a rewrite.

- **Quarterly check-in** (Cybersecurity Manager): confirm contact lists and escalation paths are current, reference the latest toolset (e.g., new platforms deployed under this roadmap — SOAR, VM platform, OT-SIEM integration), and fold in lessons-learned from any real incidents or tabletop exercises run that quarter.
- **Annual full review and formal reissue** (CISO, sign-off required): a complete walkthrough of every lifecycle phase and role assignment in Section 6.1, validated against at least one tabletop exercise run that year, with the reissued playbook version-controlled and distributed to all IR roles.
- **Ad hoc trigger**: any Sev-1/Sev-2 incident, any new critical system/platform going live, or any organizational role change affecting Section 6.2's command structure triggers an out-of-cycle playbook update regardless of where the plan sits in the quarterly/annual cycle.

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
| Mean Time to Detect (MTTD) | Downward trend; target < 1 hr for critical alerts (24x7 SOC) | Monthly |
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
| IR Playbook Currency | ≤ 365 days since last full review; quarterly check-in completed each quarter | Quarterly |

---

## 9. 12-Month Cybersecurity Roadmap

*Quick wins are front-loaded into the first 90 days per the requirement to prioritize early impact.*

### Month 1 — Foundation & Quick Wins
- **Objectives**: Establish governance rhythm; close the fastest, highest-impact gaps.
- **Projects**: Confirm/enable backup immutability; launch security awareness platform; scope the OT monitoring platform's SIEM integration (feasibility/connector assessment — the underlying OT tooling already exists); confirm M365 E5/Entra ID P2 licensing and enforce phishing-resistant MFA for all privileged accounts; validate 24x7 SOC shift-handoff and escalation procedures are documented.
- **Deliverables**: Immutable backup confirmed for Tier-1 systems; awareness platform live; OT-SIEM integration approach selected; shift-handoff runbook validated across all 3 shifts.
- **Resources**: CISO, Cybersecurity Manager, Network Specialist.
- **Outcomes**: Immediate reduction in ransomware blast-radius risk; OT-SIEM integration project chartered.

### Month 2 — Identity & Email Hardening
- **Objectives**: Close initial-access vector gaps.
- **Projects**: Deploy advanced email security (Proofpoint/Mimecast/Abnormal) as pilot; begin DLP policy design (Purview); begin secrets management pilot.
- **Deliverables**: Email security pilot live for highest-risk mailboxes (executives, finance, ops); DLP policy draft.
- **Resources**: Network Specialist, Cybersecurity Manager, GRC Consultants (policy input).
- **Outcomes**: Reduced phishing/BEC success likelihood.

### Month 3 — Vulnerability Management Stand-Up
- **Objectives**: Replace ad hoc patch visibility with a real VM program.
- **Projects**: Deploy VM platform (Tenable/Rapid7); baseline scan of full IT estate; finalize SLA policy (Section 7); align SOC workflows (CrowdStrike/QRadar alert routing) with the 3-shift rotation.
- **Deliverables**: First full vulnerability baseline report; SLA policy signed by leadership; updated alert-routing/escalation matrix for the 3-shift SOC.
- **Resources**: Cybersecurity Manager, Network Specialist, GRC Consultants.
- **Outcomes**: End of Q1 — email hardened, VM program operating, SOC workflows optimized. **90-day quick-win milestone met.**

### Month 4 — SOAR & Threat Intel
- **Objectives**: Multiply analyst capacity.
- **Projects**: Deploy SOAR integrated with QRadar; build first 3 automation playbooks (phishing triage, EDR isolation, IOC blocklist push); integrate WaterISAC + commercial TIP feed.
- **Deliverables**: SOAR live with 3 playbooks in production.
- **Resources**: SOC Analysts, Cybersecurity Manager.
- **Outcomes**: Reduced Tier-1 manual workload; faster containment.

### Month 5 — CASB/SSPM & Attack Surface Management
- **Objectives**: Close SaaS and external-exposure blind spots.
- **Projects**: Deploy CASB/SSPM; deploy ASM tool; complete DLP rollout beyond pilot.
- **Deliverables**: SaaS posture baseline report; external attack surface baseline.
- **Resources**: Network Specialist, Cybersecurity Manager.
- **Outcomes**: Shadow SaaS and internet-facing exposure now visible and tracked.

### Month 6 — IR Playbook Review & First Tabletop
- **Objectives**: Formalize governance of the existing incident response capability (not build one from scratch).
- **Projects**: Conduct the first scheduled IR playbook review (Section 6.5) and incorporate any needed updates; contract retained DFIR firm; run first tabletop (ransomware scenario) against the existing playbook.
- **Deliverables**: Reviewed/updated IR playbook with documented review cadence; DFIR retainer contract; tabletop after-action report.
- **Resources**: CISO, Cybersecurity Manager, all SOC Analysts.
- **Outcomes**: End of Month 6 — response governance, vulnerability management, and SOAR all materially matured; identity and data protection gaps closed.

### Month 7 — OT Monitoring Integration with the Central SIEM
- **Objectives**: Close the remaining OT gap — correlation and unified triage, not visibility (which already exists).
- **Projects**: Integrate the existing OT monitoring platform's alerts into QRadar for wave 1 sites; reconcile the existing OT asset inventory with Tanium/the central asset register.
- **Deliverables**: OT alerts flowing into QRadar for wave 1 sites; reconciled OT asset inventory v1.
- **Resources**: Network Specialist, SOC Analysts (OT-liaison rotation), the existing OT monitoring platform's vendor/integrator.
- **Outcomes**: OT alerts now receive the same 24x7 SOC triage discipline as IT alerts.

### Month 8 — IT/OT Segmentation Validation & Hardening
- **Objectives**: Confirm and harden the IT/OT boundary architecturally, given OT already runs on separate infrastructure.
- **Projects**: Validate/harden Panorama/ISE segmentation at the Purdue Model boundary for wave 1 sites; confirm no direct OT-to-internet paths exist; broker vendor remote access through PAM only; train SOC Analysts on OT alert triage.
- **Deliverables**: Segmentation validated and hardened at wave 1 sites; OT triage playbook and trained SOC rotation in place.
- **Resources**: Network Specialist, CISO, the OT monitoring platform's vendor/integrator.
- **Outcomes**: Materially reduced IT-to-OT lateral movement risk, confirmed rather than assumed.

### Month 9 — CNAPP/CSPM & Certificate Program Assurance
- **Objectives**: Close remaining cloud gaps; confirm certificate lifecycle coverage.
- **Projects**: Deploy CNAPP/CSPM; confirm the existing Yubico-based certificate lifecycle covers all server/service certificates (not just user/device authentication) and integrate expiry alerting into QRadar; run Purple Team exercise.
- **Deliverables**: Cloud posture baseline; confirmed certificate scope with expiry alerting live in the SIEM.
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
- **Projects**: Full security program maturity review; policy refresh (including the annual IR playbook reissue, Section 6.5); AWIA re-certification readiness check; board/executive annual report; technology refresh planning; Year 2 roadmap drafted.
- **Deliverables**: Annual report to executive leadership/board; reissued IR playbook; Year 2 roadmap.
- **Resources**: CISO, full team.
- **Outcomes**: Measurable maturity improvement from ~3.0/5 baseline (Section 1.4) further into the "Managed" range, with OT-SIEM integration, a governed IR playbook cadence, and a real VM program now permanent fixtures rather than gaps.

---

## 10. Project Plan

The full task-level project plan (phases, milestones, durations, predecessors, resource assignments, deliverables, and success criteria) is provided as two companion files:

- **`Project-Plan.csv`** — plain task table for direct import into Microsoft Project. File → Open → select CSV → map columns (Task Name, Duration, Predecessors, Resource Names) → set the project **Start Date** to the actual kickoff date; MS Project will auto-schedule all successor tasks from the Predecessors column.
- **`Project-Plan-Gantt.xlsx`** — a formatted, self-calculating Gantt chart workbook for Excel. Start/Finish dates are computed with `WORKDAY()` from a single editable **Project Start Date** cell (`G2`) and each task's predecessor; a phase-gate rule additionally holds that no task in a later phase starts until every task in the prior phase has finished, since the same core team (CISO, Cybersecurity Manager, Network Specialist, GRC Consultants) drives every phase alongside its regular 24x7 SOC duties. Task bars are shaded by phase (color-coded legend at the top) across monthly columns using conditional formatting. Edit `G2` to reschedule the entire plan — every date and bar recalculates automatically.
  - Note: at the durations given, the critical-path schedule converges in roughly 7–8 months rather than exactly 12, because it models task effort and dependencies only (no holiday or procurement/contracting lead-time buffer between phases) — and several tasks are now lighter integration/assurance work rather than ground-up deployments, since OT monitoring, 24x7 coverage, the IR playbook, and PKI already exist. Treat Section 9's month-by-month narrative above as the governance/reporting calendar, and this workbook as the underlying technical schedule; add buffer to task durations if a strict 12-month pace should be enforced.

---

## 11. NIST SP 800-53 Rev. 5 Control Mapping

Mapping of proposed technologies/processes to control families. **Status** reflects the posture *after* this roadmap's Year-1 execution, with the pre-roadmap gap noted where relevant.

| Family | Family Name | Key Controls Addressed | Addressing Technology/Process | Status After Year 1 |
|---|---|---|---|---|
| **AC** | Access Control | AC-2 (Account Mgmt), AC-3, AC-6 (Least Privilege), AC-17 (Remote Access) | Centrify/Delinea PAM (enhanced), Zscaler ZPA, Entra ID Conditional Access, quarterly recertification | Fully addressed (was: Partial — no JIT/recertification) |
| **AU** | Audit & Accountability | AU-2, AU-6 (Audit Review), AU-12 (Generation) | QRadar SIEM + SOAR + expanded log sources (OT, CASB, DLP, email) | Fully addressed (was: Partial — 24x7 review gap) |
| **CA** | Assessment, Authorization & Monitoring | CA-2 (Assessments), CA-7 (Continuous Monitoring) | GRC Consultants' quarterly control validation; CSPM/VM platforms feeding continuous monitoring | Fully addressed (was: Partial — point-in-time only) |
| **CM** | Configuration Management | CM-2, CM-6 (Config Settings), CM-8 (Component Inventory) | Tanium (IT) + existing OT-specific asset inventory/monitoring tooling (separate OT infrastructure), reconciled into a single asset register | Fully addressed for IT; **substantially addressed** for OT via existing tooling (was: OT inventory existed but reconciliation with the central register was unconfirmed) |
| **CP** | Contingency Planning | CP-9 (Backup), CP-10 (Recovery) | Immutable backup (3-2-1-1), quarterly restore tests, annual full DR test | Fully addressed (was: Partial — immutability/testing unconfirmed) |
| **IA** | Identification & Authentication | IA-2 (MFA), IA-5 (Authenticator Mgmt) | Phishing-resistant MFA via Entra ID; PAM credential rotation | Fully addressed (was: Partial) |
| **IR** | Incident Response | IR-4 (Handling), IR-6 (Reporting), IR-8 (Plan) | Existing IR playbook (Section 6) with a new confirmed annual/quarterly review cadence (Section 6.5), SOAR playbooks, retained DFIR firm | Fully addressed (was: Substantially met — playbook already existed but lacked a confirmed review cadence) |
| **MA** | Maintenance | MA-2, MA-4 (Remote Maintenance) | PAM-brokered vendor/OT remote maintenance access (no standing VPN) | Fully addressed (was: Not addressed for OT vendor access) |
| **MP** | Media Protection | MP-5 (Media Transport), MP-7 (Media Use) | DLP policies extended to removable media/egress channels | Substantially improved (was: Not addressed) |
| **PE** | Physical & Environmental Protection | PE-3, PE-6 | Out of scope for this cyber-focused roadmap — recommend confirming existing physical security program at treatment/pumping facilities aligns with OT criticality | Not addressed by this roadmap (flag for separate physical security review) |
| **PL** | Planning | PL-2 (System Security Plans), PL-8 (Architecture) | Section 3 architecture; annual policy review | Fully addressed (was: Partial) |
| **PS** | Personnel Security | PS-3 (Screening), PS-4 (Termination) | IAM access review tied to HR termination feed (weekly cadence, Section 5.2) | Substantially improved (was: Partial — manual/ad hoc) |
| **RA** | Risk Assessment | RA-3 (Risk Assessment), RA-5 (Vulnerability Monitoring) | Dedicated VM platform + risk register (GRC-owned) | Fully addressed (was: Partial — no continuous scanning) |
| **SA** | System & Services Acquisition | SA-9 (External Systems), SA-22 (Unsupported Components) | OT vendor risk review (Section 5.4), SolarWinds supply-chain governance | Substantially improved (was: Not formally addressed) |
| **SC** | System & Communications Protection | SC-7 (Boundary Protection), SC-8 (Transmission Confidentiality) | Panorama/ISE IT/OT segmentation (validated/hardened), Yubico-based PKI/certificate lifecycle mgmt | Fully addressed for IT; **substantially improved** for OT boundary (was: Segmentation existed via separate OT infrastructure, but formal validation/monitoring integration was unconfirmed) |
| **SI** | System & Information Integrity | SI-3 (Malicious Code), SI-4 (Monitoring), SI-7 (Integrity) | CrowdStrike + existing OT monitoring platform (integrated into QRadar) + FIM on critical servers | Fully addressed (was: OT anomaly detection existed via separate tooling, but SIEM integration and FIM coverage were unconfirmed) |

**Not fully addressed after Year 1 (carry into Year 2 roadmap):** PE (physical security review recommended as a separate, facilities-partnered workstream); SR (Supply Chain Risk Management) formalization beyond the SolarWinds/OT-vendor reviews started in Year 1; PM (Program Management) family formal documentation (security program charter, resource plan) as a Year 2 documentation exercise once operational gaps are closed.

---

## Appendix: Staffing Recommendation Summary

The technology roadmap above assumes the **existing team — including the already-staffed 24x7 SOC (3 shifts, minimum 2–3 SOC Analysts per shift) and the existing IR playbook** — absorbs the work described, augmented only by a retained DFIR firm for surge forensics support (Section 6.3). No large internal hiring wave or managed-detection contract is required to execute this roadmap, since the coverage-hours and OT-visibility gaps a typical utility this size would need to close from scratch are already closed.

The incremental headcount below is not required to execute this roadmap, but would give dedicated, specialized ownership to functions the current team covers only implicitly alongside their other duties:

- 1 dedicated Vulnerability Management Analyst
- 1 dedicated IAM/PAM Administrator
- 1 Security Architect
- Optionally, 1 OT-SIEM Integration Engineer (or a confirmed liaison/SLA model with whichever team already operates the OT monitoring tooling) to own that integration long-term rather than as a one-time project

Treat this list as a Year 2 hiring conversation once the Year 1 gaps above (vulnerability management program, OT-SIEM integration, SOAR, IR playbook governance) are closed with the existing team.
