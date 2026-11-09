# Privacy policy: SLA Breach Post-Mortem & Root Cause Gate for JSM

Updated 9 November 2026

This policy describes the Atlassian Marketplace app **SLA Breach Post-Mortem & Root Cause Gate for JSM** ("the app"), made by EvidencePair LLC ("EvidencePair", "we"). EvidencePair's company privacy policy, which covers our website and email you send us, is at <https://evidencepair.com/privacy/>.

## In short

- The app runs on Atlassian Forge, inside your Jira site. It declares no external egress and no remote services, and it makes requests only to your site's Jira and Jira Service Management APIs.
- It keeps its data in Jira itself, as two Jira entity properties. That data is never sent to EvidencePair.
- The only personal data it stores itself is the Atlassian account ID of the person who signs a post-mortem, plus whatever people type into a post-mortem's text fields.

## What the app stores

**One project property, `slaGatePolicy`, per project that uses the app:**

- the project's key and ID, and its service desk ID;
- the policy: which SLAs need a post-mortem, the root-cause categories people can choose, whether an impact summary is required, the minimum length of the preventive action plan, and whether the workflow validator blocks resolution;
- test markers for simulated SLA breaches, if someone used that feature: the issue key, the SLA name, the elapsed time shown, and when it was flagged;
- when the policy was last updated.

**One issue property, `slaBreachPostMortem`, per issue with a signed post-mortem:**

- the issue key and the project ID;
- the Atlassian account ID of the person who signed it, and when;
- the SLA name, the root-cause category, the impact summary and the preventive action plan, as typed;
- whether the breach was a simulated one;
- a SHA-256 checksum of the account ID, issue key, SLA name, root-cause category, time and preventive action plan, which lets the app tell whether those values have changed since the record was signed. It does not cover the impact summary, and it adds no information of its own.

The app does not look up names, email addresses or profiles. Text that people type into a post-mortem can contain personal data, and the app stores it exactly as entered.

## What the app reads from Jira

- The issue or project the app is opened from.
- The issue's SLA cycles, from Jira Service Management.
- The two properties above.
- On the project page, the 15 most recently created issues in the project: key, summary, status and issue type, to show which have a post-mortem.

It reads no issue descriptions, comments or attachments.

## Where the data lives

Entity properties are part of your Jira site's data. Atlassian stores them with the rest of your site, in your site's data residency location, under Atlassian's own security and privacy commitments. The app has no storage of its own: it uses no Forge storage and no Forge SQL, and EvidencePair runs no servers in its path.

## Who can see it

Jira shows an entity property to anyone who can view the issue or project it belongs to, through Jira's REST API. Anyone who can see an issue can therefore read its post-mortem, including the signer's account ID. Jira's permissions also decide who can change the properties: changing an issue property needs permission to edit the issue, and changing a project property needs permission to administer the project.

## Retention and deletion

The app keeps both properties until someone deletes them. It has no retention setting, deletes nothing on its own schedule, and has no uninstall step. Deleting an issue deletes its properties. A Jira administrator can delete either property at any time through Jira's REST API, and should do so before uninstalling if the data must go.

## Account closure

The app does not take part in Atlassian's personal data reporting. When an Atlassian account is closed, its account ID stays in any post-mortem that person signed until that property, or the issue, is deleted.

## Platform logs

The app writes nothing to the Forge platform's logs. Atlassian's platform can still record errors it sees while the app runs, such as an unhandled error and its message ([Atlassian: view app logs](https://developer.atlassian.com/platform/forge/view-app-logs/)).

## Email and support

If you email us, our [company privacy policy](https://evidencepair.com/privacy/) describes how we handle that email, and the current restrictions on where our apps may be used.

## What we never do

We do not sell personal data or use it for unrelated advertising or AI training. The app's data stays in your Jira site and never reaches us.

## Changes to this policy

The date at the top changes when this policy does. Every version is kept in this repository's history.

## Contact

hello@evidencepair.com
