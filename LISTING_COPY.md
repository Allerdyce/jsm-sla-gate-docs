# Atlassian Marketplace Listing Copy

- **App Name:** SLA Breach Post-Mortem & Root Cause Gate for JSM
- **App Key:** `com.evidencepair.jsm.sla.gate`
- **Tagline (Short Description):** Block resolving SLA-breached JSM requests until a SHA-256 signed Root Cause & CAPA Post-Mortem is recorded.
- **Categories:** IT & Helpdesk, Workflow, Compliance & Security

---

## Full Marketplace Description

Prevent silent SLA breaches from being closed without accountability or engineering follow-through. **SLA Breach Post-Mortem & Root Cause Gate for JSM** combines live Jira Service Management SLA telemetry, a structured 5-Whys Root Cause & Corrective/Preventive Action (CAPA) issue panel, and a native Jira Workflow Validator into a 100% *Runs on Atlassian* compliance gate.

### Why Incident & Service Desk Teams Choose SLA Breach Gate

1. **Automated JSM SLA Breach Detection (`jira:issuePanel` & `jira:workflowValidator`)**
   - Inspects live JSM SLA clocks (`/rest/servicedeskapi/request/{issueKey}/sla`) including *Time to first response* and *Time to resolution*.
   - Tickets that meet their SLA goals resolve normally with zero extra friction (`CLEARED_NO_SLA_BREACH`).

2. **Structured 5-Whys Root Cause & CAPA Attestation (`jsm-sla-postmortem-panel`)**
   - When a target SLA breaches, agents complete the Root Cause Category (*Infrastructure*, *Deployment Regression*, *Third-Party Vendor*, *Capacity/Staffing*, *Configuration Drift*), Customer Impact Summary, and Preventive Action Plan (CAPA).
   - Clicking **Sign & Lock Post-Mortem** computes a deterministic cryptographic digest: `SHA-256(accountId|issueKey|slaName|rootCauseCategory|timestamp|capaSummary)`.

3. **Tamper-Evident Workflow Resolution Validator (`jsm-sla-resolution-validator`)**
   - Blocks resolving breached JSM requests (`BLOCKED_BY_UNSIGNED_SLA_BREACH`) until a valid, SHA-256 verified Post-Mortem is locked on the issue (`CLEARED_FOR_RESOLUTION_SHA256_VERIFIED`).

4. **Space Audit Radar & 1-Click ISO 27001 / ITIL / SOC2 CSV Export (`jsm-sla-gate-admin`)**
   - Space admins configure target SLAs and minimum CAPA lengths and export a full RFC-4180 CSV audit ledger in 1 click.

5. **100% Runs on Atlassian — Zero Forge SQL/KVS, Zero External Egress**
   - Stores all policy and post-mortem records in native Jira Project & Issue Entity Properties (`<32 KB` enforced).

---

## Version Highlights

### Highlight 1: Structured 5-Whys Root Cause & SHA-256 CAPA Issue Panel
Capture Root Cause Category, Customer Impact Summary, and Corrective & Preventive Action (CAPA) directly on SLA-breached JSM requests with a deterministic SHA-256 Incident Commander signature stored in Jira Issue Property `slaBreachPostMortem`.

### Highlight 2: Native Jira Workflow Validator Resolution Gate
Automatically block resolving SLA-breached JSM requests (`BLOCKED_BY_UNSIGNED_SLA_BREACH`) while allowing non-breached tickets and SHA-256 verified post-mortem tickets (`CLEARED_FOR_RESOLUTION_SHA256_VERIFIED`) to transition seamlessly.

### Highlight 3: Space SLA Gate Policy & 1-Click ISO/ITIL/SOC2 Audit CSV Export
Configure enforced JSM SLAs and minimum CAPA requirements per space (`slaGatePolicy` `<32 KB`) and export your project-wide breached ticket CAPA ledger to RFC-4180 CSV in 1 click.
