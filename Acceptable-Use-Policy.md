# Acceptable Use Policy
### Water District — Information Systems, Data, and Artificial Intelligence Tools

**Policy Owner:** Chief Information Security Officer (CISO)
**Effective Date:** [Insert Effective Date]
**Applies To:** All employees, contractors, temporary staff, interns, board members, and third parties granted access to Water District information systems, networks, devices, or data
**Review Cadence:** Annually, per Section 5.5 of the Enterprise Cybersecurity Strategy & Roadmap, or immediately upon a material change in approved tooling
**Classification:** Internal — Distribute to all personnel; acknowledgment required

---

## 1. Purpose

This policy defines acceptable use of Water District's information systems, networks, devices, and data, and establishes the rules governing the use of Artificial Intelligence (AI) tools specifically. As a water utility and critical infrastructure operator, Water District's systems support treatment, distribution, and public-safety operations; misuse of IT resources — including well-intentioned but unauthorized use of AI tools — carries operational, regulatory, and public-safety risk beyond a typical enterprise.

This policy supports the following control families and frameworks referenced throughout Water District's Enterprise Cybersecurity Strategy & Roadmap: NIST SP 800-53 Rev. 5 **PL-4 (Rules of Behavior)** and **AC-20 (Use of External Systems)**, and CIS Critical Security Controls **Control 3 (Data Protection)** and **Control 9 (Email and Web Browser Protections)**.

## 2. Scope

This policy applies to:
- All Water District-owned or -managed devices (workstations, laptops, mobile devices, servers, OT/engineering workstations).
- All Water District networks, cloud services (Microsoft 365, any IaaS/SaaS), and accounts, whether accessed on-site, remotely (via Zscaler ZPA), or on a personal device.
- Any third-party website, application, browser extension, or service — including all Artificial Intelligence tools — accessed using Water District credentials, devices, or network connectivity, or into which Water District data is entered from any device or account.

## 3. General Policy Statement

### 3.1 Authorized Use

Water District information systems are provided for legitimate business purposes. Incidental, reasonable personal use is permitted provided it does not interfere with job performance, consume significant resources, violate any provision of this policy, or expose Water District to legal, regulatory, or security risk.

### 3.2 Account & Credential Responsibility

- Users are responsible for all activity under their credentials. Credentials, including MFA devices/YubiKeys, must never be shared, written down in an accessible location, or reused across personal and work accounts.
- Any suspected compromise of a credential must be reported immediately per Section 5.

### 3.3 Prohibited Activities (General)

Without prior written authorization from the CISO's office, users must not:
- Attempt to access systems, data, or accounts beyond their authorized role (least privilege applies — see the Cybersecurity Strategy, Section 3.1).
- Disable, bypass, or attempt to circumvent security controls (EDR, DLP, MFA, web/DNS filtering, PAM session recording, etc.).
- Install unauthorized software, browser extensions, or hardware (including personal USB storage) on Water District devices.
- Connect unauthorized personal devices to OT/ICS networks or systems under any circumstance.
- Use Water District systems to engage in illegal activity, harassment, or activity that damages Water District's reputation.
- Exfiltrate, forward to personal accounts, or store Water District data (especially OT/SCADA design data, PII, or credentials) on unapproved personal cloud storage, USB media, or third-party services — **including AI tools, per Section 4.**

### 3.4 Software, Hardware & Cloud Service Installation

All software, browser extensions, cloud/SaaS services, and hardware must be approved through IT/Cybersecurity prior to use on Water District devices or with Water District data, and are subject to the Vulnerability Management (Section 7) and Shadow IT/Shadow AI Discovery (Section 3.13) processes described in the Cybersecurity Strategy. Unauthorized (“shadow IT”) tools are subject to removal/blocking without advance notice once discovered.

### 3.5 Data Classification & Handling

Data must be handled according to its classification (Public, Internal, Confidential, Restricted — Restricted includes OT/SCADA network diagrams and control-system data, customer PII, and credentials). Confidential and Restricted data may only be stored, processed, or transmitted through approved Water District systems and services. **This restriction explicitly includes AI tools — see Section 4.**

### 3.6 Removable Media

Removable media use on OT/ICS systems requires documented CISO approval and pre-use malware scanning through an isolated scanning station; general enterprise removable-media use follows the DLP policies referenced in the Cybersecurity Strategy, Section 3.9.

### 3.7 Remote Access & BYOD

All remote access must use Zscaler ZPA with an approved, compliant, MFA-enrolled device. Personal (BYOD) devices accessing Water District email or data must meet minimum security baseline (current OS, disk encryption, screen lock) as enforced by mobile device management.

### 3.8 Email, Messaging & Social Media

Users must not use Water District email or messaging systems to transmit Confidential or Restricted data to unauthorized recipients or services (including pasting such data into an AI chat prompt via email/Teams integrations). Official Water District communications on social media are restricted to designated communications staff.

### 3.9 Physical Security

Devices must be locked when unattended, not left visibly unattended in vehicles or public spaces, and physical access to OT/ICS facilities and equipment follows site-specific physical security procedures independent of this policy.

### 3.10 Monitoring & No Expectation of Privacy

By using Water District systems, users consent to monitoring consistent with this policy and the Cybersecurity Strategy's Security Operations Plan (Section 5) and Shadow IT/Shadow AI Discovery program (Section 3.13), including but not limited to SIEM (QRadar), EDR (CrowdStrike), DNS/web filtering (Cisco Umbrella), CASB/SSPM, DLP (Microsoft Purview), and identity/OAuth logs (Entra ID). Users should have no expectation of privacy in their use of Water District systems, devices, or accounts.

---

## 4. Artificial Intelligence (AI) Tools — Special Provisions

### 4.1 Current Status: No AI Tools Are Currently Approved

**As of the effective date of this policy, no third-party Artificial Intelligence or Machine Learning tool of any kind is approved for use with Water District data, systems, or accounts.** This includes, without limitation, public generative AI chatbots, AI-powered browser extensions and productivity add-ins, AI coding assistants, AI meeting-notetaker/transcription bots, and locally installed or self-hosted AI/LLM software. This is the default position until a specific tool completes the review process in Section 4.4 and is published on the **Approved AI Tools Registry**. Absence from the Registry means the tool is unauthorized, regardless of how widely it is used elsewhere or how harmless it may seem.

### 4.2 Prohibited AI-Related Activities

Unless and until a specific tool is added to the Approved AI Tools Registry, users must not:

1. Enter any Water District Confidential or Restricted data — including but not limited to customer PII, employee data, credentials, API keys, network diagrams, OT/SCADA design or configuration data, incident details, vulnerability findings, financial data, or any content marked "Internal" or higher — into any public or unapproved AI tool, whether via a web interface, browser extension, API, or an AI feature embedded in another product (e.g., an AI "assistant" feature bundled into unrelated software).
2. Install or enable an AI browser extension, plugin, or add-in on any Water District device.
3. Connect or authenticate an AI tool to a Water District account via OAuth/SSO ("Sign in with Microsoft/Google") or any API key tied to Water District systems.
4. Use a personal AI tool account or subscription to perform Water District work, or use a Water District account/device to access a personal AI subscription for work purposes.
5. Install, run, or connect to a local or self-hosted AI/LLM tool (e.g., Ollama, LM Studio, or similar) on any Water District device or network, including OT/engineering workstations, without prior written CISO approval.
6. Use AI-generated code, configuration, or content in any production system — IT or OT — without the same security review, testing, and change-management process required of any other code or configuration change.
7. Rely on an AI meeting assistant, transcription bot, or note-taker in any meeting where Confidential or Restricted topics (security incidents, vulnerabilities, OT operations, personnel matters) are discussed.

### 4.3 Examples of Covered Tools (Non-Exhaustive)

This policy covers, by way of example and without limitation: ChatGPT/ChatGPT Enterprise (unless separately approved), Claude.ai, Google Gemini, Microsoft Copilot (consumer tier — see note below), Perplexity, Midjourney and other image generators, GitHub Copilot and other AI coding assistants, Otter.ai and other AI transcription tools, and general-purpose AI browser extensions (e.g., "Merlin," "Sider," "Monica," or similar). The absence of a specific tool from this list does not imply approval — see Section 4.1.

> **Note on Microsoft Copilot:** Water District's existing Microsoft 365 investment may include a Copilot tier in the future. Until Cybersecurity formally confirms tenant-level data-handling configuration (no training on Water District data, appropriate license tier, Purview DLP/sensitivity-label enforcement extended to Copilot) and adds it to the Approved AI Tools Registry, Copilot remains unapproved and subject to this policy like any other AI tool.

### 4.4 Approved AI Tools Registry & Exception Request Process

1. Any employee, team, or vendor wishing to use a specific AI tool must submit a request to Cybersecurity (via the IT service desk, ticket category "AI Tool Review") describing the tool, intended use case, and data types involved.
2. Cybersecurity (Cybersecurity Manager, supported by GRC Consultants) reviews the request against, at minimum: the vendor's data-handling and training-data terms (does the vendor train on submitted data — an enterprise/opt-out agreement is required), applicable certifications (SOC 2 Type II, ISO 27001, or equivalent), a signed Data Processing Agreement where Water District data would be processed, integration/authentication method, and alignment with data classification rules in Section 3.5.
3. Approved tools are published on the **Approved AI Tools Registry** (maintained by the CISO's office, reviewed at minimum quarterly alongside the Shadow IT/Shadow AI Discovery review — Cybersecurity Strategy Section 5.2) along with any conditions of use (e.g., approved for Internal data only, specific business units, specific licensed tier).
4. Requests are typically resolved within 10 business days; urgent business needs may be escalated to the CISO directly.
5. Approval is tool-and-configuration-specific, not blanket: e.g., approval of an enterprise-licensed AI tool with data-training disabled does not extend to the same vendor's free/consumer tier.

### 4.5 Monitoring of AI Tool Usage

Consistent with Section 3.10 and the Cybersecurity Strategy's Section 3.13 (Shadow IT / Shadow AI Discovery & Governance), Water District actively monitors for AI tool usage via DNS/web category filtering (Cisco Umbrella, Palo Alto Panorama), CASB/SSPM generative-AI app categories once deployed, Entra ID OAuth consent/Cloud App Discovery logs, endpoint software/browser-extension inventory (Tanium, CrowdStrike), and DLP policies scoped to browser paste/upload actions (Microsoft Purview). Detected use of an unapproved AI tool may result in automated blocking, a coaching message directing the user to this policy and the exception process, manager notification, and — for confirmed data exposure — incident response handling per the Cybersecurity Strategy's Incident Response Plan (Section 6), including potential regulatory notification obligations.

### 4.6 Special Considerations for OT/ICS Environments

Given Water District's separate OT/ICS infrastructure, AI tools are subject to additional restriction in that environment: no AI tool of any kind — approved or otherwise — may be installed, executed, or connected to any OT/ICS network, engineering workstation, HMI, or historian without explicit written CISO approval and a documented risk assessment, regardless of whether the same tool is separately approved for general IT use.

---

## 5. Incident Reporting Obligation

Any user who becomes aware of a violation of this policy — their own or another's, including inadvertent entry of sensitive data into an AI tool — must report it immediately to the Cybersecurity Manager or via the incident reporting channel described in the Cybersecurity Strategy's Incident Response Plan (Section 6). Prompt self-reporting of an inadvertent violation is considered a mitigating factor in any disciplinary review under Section 6 below.

## 6. Enforcement & Disciplinary Action

Violations of this policy — general or AI-specific — are subject to disciplinary action up to and including termination of employment or contract, consistent with Water District's HR policies, and may result in immediate suspension of system access pending investigation. Violations resulting in exposure of Confidential/Restricted data, regulatory data, or OT/SCADA information will additionally be handled through the Incident Response Plan (Cybersecurity Strategy, Section 6) and may trigger regulatory or contractual notification obligations.

## 7. Policy Review & Ownership

This policy is owned by the CISO and reviewed at minimum annually (Cybersecurity Strategy, Section 5.5), or immediately upon: addition of a tool to the Approved AI Tools Registry, a material AI-related incident, or a material change to the underlying technology stack referenced in Sections 3.10 and 4.5. Version history is maintained by the CISO's office.

## 8. Related Documents

- Enterprise Cybersecurity Strategy & Roadmap (`Cybersecurity-Strategy-and-Roadmap.md`) — particularly Section 3.13 (Shadow IT/Shadow AI Discovery), Section 6 (Incident Response Plan), and Section 4 (Missing Products & Licensing, CASB/SSPM item).
- NIST SP 800-53 Rev. 5: PL-4 (Rules of Behavior), AC-20 (Use of External Systems).
- CIS Critical Security Controls: Control 3 (Data Protection), Control 9 (Email and Web Browser Protections).

---

## 9. Acknowledgment

I have read, understand, and agree to comply with this Acceptable Use Policy, including the Artificial Intelligence Tools provisions in Section 4. I understand that no AI tools are currently approved for use with Water District data or systems, that this restriction applies regardless of a tool's popularity or perceived harmlessness, and that my use of Water District systems is subject to monitoring as described above.

| Field | |
|---|---|
| Employee Name (Print) | ________________________________ |
| Signature | ________________________________ |
| Date | ________________________________ |
| Department | ________________________________ |
