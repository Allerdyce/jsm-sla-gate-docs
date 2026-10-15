# SLA Breach Post-Mortem & Root Cause Gate for JSM — User & Administrator Guide

## 1. Installation & Quick Start

1. Install **SLA Breach Post-Mortem & Root Cause Gate for JSM** from the Atlassian Marketplace into your Jira / Jira Service Management Cloud site.
2. Open any Jira Service Management project (e.g., **IT Helpdesk**) and select **SLA Breach Gate & Audit Radar** in the left project sidebar (`jira:projectPage`).
3. Review the active Space SLA Gate Policy:
   - **Strict Workflow Resolution Gate**: Enabled by default (`true`).
   - **Enforced JSM SLAs**: `Time to first response`, `Time to resolution`.
   - **Minimum CAPA Preventive Plan Length**: Default `20` characters.
4. Click **Save Space SLA Gate Policy (<32 KB)** to persist the policy directly to `/rest/api/3/project/{projectId}/properties/slaGatePolicy`.

---

## 2. Attaching the Workflow Resolution Validator (`jsm-sla-resolution-validator`)

To block agents from resolving SLA-breached requests without a signed post-mortem:

1. Navigate to **Project Settings → Workflows** and edit your JSM request workflow.
2. Select the transition leading to your resolved status (e.g., **Resolve Issue** or **Done**).
3. Click **Validators → Add validator**.
4. Select **SLA Breach Post-Mortem Signed Validator** (`jsm-sla-resolution-validator`) and publish the workflow draft.

### How the Validator Evaluates Transitions
- **`CLEARED_NO_SLA_BREACH`**: If none of the enforced SLAs on the request have breached, the transition succeeds immediately (`{ result: true }`).
- **`BLOCKED_BY_UNSIGNED_SLA_BREACH`**: If an enforced SLA has breached and no SHA-256 verified post-mortem exists in Issue Property `slaBreachPostMortem`, the transition is blocked with a clear instruction message.
- **`CLEARED_FOR_RESOLUTION_SHA256_VERIFIED`**: Once an Incident Commander signs and locks the Post-Mortem & CAPA Gate panel, the validator verifies the SHA-256 digest and allows the resolution transition (`{ result: true }`).

---

## 3. Completing & Signing an SLA Breach Post-Mortem on an Issue

1. Open the breached JSM request and locate the **SLA Breach Post-Mortem & CAPA Gate** panel (`jira:issuePanel`).
2. Review the live JSM SLA clock table fetched from `/rest/servicedeskapi/request/{issueKey}/sla`.
3. Complete the three mandatory fields:
   - **Root Cause Category**: Choose `Infrastructure`, `Deployment Regression`, `Third-Party Vendor`, `Capacity/Staffing`, or `Configuration Drift`.
   - **Customer Impact & 5-Whys Summary**: Document customer blast radius and root cause chain.
   - **Corrective & Preventive Action Plan (CAPA)**: Document concrete remediation steps (minimum 20 characters).
4. Click **Sign & Lock Post-Mortem (SHA-256)**.
5. The app computes the canonical digest `SHA-256(accountId|issueKey|slaName|rootCauseCategory|timestamp|capaSummary)` and locks the record in `/rest/api/3/issue/{issueKey}/properties/slaBreachPostMortem`.

---

## 4. Exporting the Space-Wide ISO 27001 / ITIL / SOC2 CSV Audit Ledger

1. Open **SLA Breach Gate & Audit Radar** from the project sidebar.
2. Review the **Space-Wide SLA Breach Post-Mortem & CAPA Audit Ledger** table.
3. Click **1-Click Export Audit Ledger CSV** to generate an RFC-4180 compliant CSV containing issue keys, breached SLAs, root cause categories, CAPA summaries, signer Account IDs, UTC timestamps, and 64-character SHA-256 signatures.

---

## 5. Support & SLA

For technical support, feature requests, or security inquiries, contact **EvidencePair LLC** at `support@titlecovenant.com`.
