# SLA Breach Post-Mortem & Root Cause Gate for JSM

**Marketplace Title:** SLA Breach Post-Mortem & Root Cause Gate for JSM  
**App Key:** `com.evidencepair.jsm.sla.gate`  
**App ID:** `ari:cloud:ecosystem::app/ad9771a0-9b35-45c3-a169-8931e3435fa1`  
**Developer / Vendor:** EvidencePair LLC  
**Support & Security Contact:** `support@titlecovenant.com`  
**Runtime:** Atlassian Forge `nodejs24.x` (100% *Runs on Atlassian* — Zero External Egress, Zero Forge SQL/KVS)

---

## Overview

**SLA Breach Post-Mortem & Root Cause Gate for JSM** enforces structured Root Cause Analysis (5-Whys classification), Corrective & Preventive Action (CAPA) documentation, and cryptographic **SHA-256 Incident Commander Sign-Off** before SLA-breached Jira Service Management (JSM) requests can transition to a resolved workflow state.

### Key Capabilities

1. **Space SLA Gate Policy & Breach Detector (`jira:projectPage` — `jsm-sla-gate-admin`)**
   - Configure which JSM SLA clocks (e.g., *Time to first response*, *Time to resolution*) require mandatory post-mortems upon breach.
   - Enforce minimum CAPA preventive action plan length and strict workflow transition blocking.
   - Stored in Jira Project Property `slaGatePolicy` (`<32 KB` enforced).

2. **Structured Root Cause & SHA-256 Post-Mortem Issue Panel (`jira:issuePanel` — `jsm-sla-postmortem-panel`)**
   - Displays real-time JSM SLA clock status (`/rest/servicedeskapi/request/{issueKey}/sla`).
   - Captures structured Root Cause Category (`Infrastructure`, `Deployment Regression`, `Third-Party Vendor`, `Capacity/Staffing`, `Configuration Drift`), Customer Impact / 5-Whys Summary, and CAPA Preventive Action Plan.
   - Computes a deterministic SHA-256 signature (`accountId|issueKey|slaName|rootCauseCategory|timestamp|capaSummary`) stored in Jira Issue Property `slaBreachPostMortem` (`<32 KB` enforced).

3. **Workflow Resolution Gate & Space Audit CSV Export (`jira:workflowValidator` — `jsm-sla-resolution-validator`)**
   - Blocks resolving SLA-breached JSM requests (`BLOCKED_BY_UNSIGNED_SLA_BREACH`) until a valid, tamper-verified SHA-256 Post-Mortem record is present (`CLEARED_FOR_RESOLUTION_SHA256_VERIFIED`).
   - Provides a space-wide **SLA Breach Post-Mortem Audit Ledger** with 1-click **ISO 27001 / ITIL / SOC2 CSV Export**.

---

## Documentation Index

- [User & Administrator Guide](./USER_GUIDE.md)
- [Privacy, Data Residency & Security Architecture](./PRIVACY_AND_SECURITY.md)
- [End-User License Agreement (EULA)](./EULA.md)
- [Atlassian Marketplace Listing Copy](./LISTING_COPY.md)
- [Cloud Per-User Pricing Tiers](./PRICING_TABLE.md)
