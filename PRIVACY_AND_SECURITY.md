# Privacy, Data Residency & Security Architecture

**App Title:** SLA Breach Post-Mortem & Root Cause Gate for JSM  
**App Key:** `com.evidencepair.jsm.sla.gate`  
**Vendor:** EvidencePair LLC  
**Security Contact:** `support@titlecovenant.com`

---

## 1. 100% Runs on Atlassian — Zero External Data Egress

**SLA Breach Post-Mortem & Root Cause Gate for JSM** runs exclusively inside Atlassian's managed Forge runtime (`nodejs24.x`).
- **Zero `remotes:` declarations** in `manifest.yml`.
- **Zero external HTTP/HTTPS calls**, webhooks, analytics trackers, or third-party servers.
- All data remains strictly within your Atlassian Cloud tenant boundary and inherits your Jira / Jira Service Management data residency controls.

---

## 2. Host-Native Entity Property Storage (<32 KB Enforced, Zero Forge SQL/KVS)

This application does **not** use external databases, Forge SQL, or Forge KVS. All configuration and attestation metadata is stored directly inside your Jira Cloud instance using native Jira Entity Properties:

| Property Key | REST v3 Endpoint | Purpose | Size Guard |
| :--- | :--- | :--- | :--- |
| `slaGatePolicy` | `/rest/api/3/project/{projectId}/properties/slaGatePolicy` | Space-level SLA post-mortem enforcement rules and required categories | `<32 KB` (`32,768` bytes UTF-8) |
| `slaBreachPostMortem` | `/rest/api/3/issue/{issueKey}/properties/slaBreachPostMortem` | Ticket-level Root Cause Category, Impact Summary, CAPA Plan, and SHA-256 signature | `<32 KB` (`32,768` bytes UTF-8) |

Every write operation passes through `assertPropertyPayloadUnder32KB(payload)` prior to persistence.

---

## 3. Personal Data & Privacy Policy

- **No Personal Data Collection:** The app does not collect, transmit, or store personal data outside Atlassian Cloud.
- **Account ID Attribution:** When an Incident Commander signs a post-mortem, their opaque Atlassian `accountId` (e.g., `712020:...`) and the UTC timestamp are recorded inside the issue's `slaBreachPostMortem` property solely for internal audit attribution within your Jira site.
- **Uninstallation & Data Deletion:** Because all data is stored in host Jira Project and Issue Entity Properties, customer admins retain full read/delete control via Jira REST API v3 at all times.

---

## 4. Least-Privilege Forge Scopes

- `read:jira-work` — Read project issues and entity properties (`slaGatePolicy`, `slaBreachPostMortem`).
- `write:jira-work` — Write signed post-mortem records to Jira Issue Properties (`slaBreachPostMortem`).
- `manage:jira-project` — Save space-level SLA Gate Policy rules to Jira Project Properties (`slaGatePolicy`).
- `read:servicedesk-request` — Inspect live JSM SLA clock cycles via `/rest/servicedeskapi/request/{issueIdOrKey}/sla`.
- `read:app-data:jira` / `write:app-data:jira` — Read/write app-scoped Jira metadata.

---

## 5. Vulnerability Disclosure & Security Contact

To report a security issue or request compliance documentation (SOC 2 / ISO 27001 / CAIQ), email `support@titlecovenant.com`. Security inquiries receive an initial response within 24 hours.
