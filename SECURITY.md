# Security: SLA Breach Post-Mortem & Root Cause Gate for JSM

Updated 10 November 2026

How the Atlassian Marketplace app **SLA Breach Post-Mortem & Root Cause Gate for JSM**, made by EvidencePair LLC, handles data, what protects it, how we handle vulnerabilities, and which certifications apply. What the app stores is listed in full in its [privacy policy](./PRIVACY_POLICY.md).

## Data handling

- **Forge only.** The app runs on Atlassian Forge (`nodejs24.x`) on Atlassian infrastructure. Atlassian lists it as eligible for the Runs on Atlassian program.
- **No egress.** The manifest declares no external permissions and no remote services. The app makes requests only to your site's Jira and Jira Service Management APIs, and it loads no analytics, error-reporting services or external assets.
- **No storage of its own.** The app keeps its data in two Jira entity properties on your site (`slaGatePolicy` on a project, `slaBreachPostMortem` on an issue). It uses no Forge storage or Forge SQL, and EvidencePair has no copy.
- **Requests run as the user where there is one.** The app's screens call Jira as the person using them, so Jira applies that person's own permissions. They work only on the issue or project they're opened from, as Jira reports it. If Jira doesn't say which one that is, they show a message rather than reading anything else. The workflow validator runs without a signed-in user, so it reads, as the app, only what it needs: the issue's SLA cycles and the two properties. It writes nothing.
- **Permissions it asks for.**
  - `read:jira-work`: read issues, the recent-issues list and the two properties.
  - `write:jira-work`: save a signed post-mortem to its issue.
  - `manage:jira-project`: save the project's policy.
  - `read:servicedesk-request`: read an issue's SLA cycles.
  - `read:app-data:jira`, `write:app-data:jira`: read and write app data in Jira.
- **Who signs.** The account ID on a post-mortem is the person signed in to Jira, as Forge reports it. The app ignores any account ID the screen sends.
- **What the checksum is.** Each post-mortem carries a SHA-256 checksum of its account ID, issue key, SLA name, root-cause category, time and preventive action plan (not the impact summary). It shows whether the stored record still matches what was signed. It is not a digital signature: someone with permission to edit the issue property could change the record and recompute the checksum.

## Encryption

The app stores nothing outside your Jira site and sends nothing outside Atlassian's platform. Encryption in transit and at rest is provided by Atlassian as part of your site, as described in the [Atlassian Trust Center](https://www.atlassian.com/trust). The app adds no encryption of its own.

## Vulnerability management

- **Reporting.** Email security@evidencepair.com. Tell us the app, what you found and how to reproduce it. Please do not test against sites you do not own. We acknowledge every report within one business day.
- **Response.** EvidencePair adopted a written security incident response plan on 12 October 2026. It covers triage, containment, investigation, notification of Atlassian and affected customers, and recovery.
- **Dependencies.** The app's npm dependencies are pinned in its lockfile. Atlassian maintains and patches the Forge runtime.
- **Bug bounty.** We do not run a bug bounty programme.

## Certifications

EvidencePair holds no security certifications, and the app has had no independent penetration test or external audit. The infrastructure the app runs on is Atlassian's, and Atlassian's own certifications cover it: see the [Atlassian Trust Center](https://www.atlassian.com/trust).
