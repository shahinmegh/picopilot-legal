# PI Copilot — Privacy Policy

**Last updated:** 21 September 2026
**Published by:** Onreera
**Contact:** info@onreera.com

PI Copilot is a capacity and velocity planner for SAFe Program Increments, built
as an Atlassian Forge app for Jira Cloud.

This policy exists because the app handles information about named employees —
including when they are off sick. It is written to be read by the administrator
who has to approve the install, not to be skimmed.

## The short version

**Onreera never receives your data.** The app runs entirely inside Atlassian's
infrastructure and cannot make a network call to anywhere else. There is no
server of ours for your data to reach. Everything below is detail on that one
fact.

This is not a promise about our intentions. A Forge app must declare every
outbound destination in its manifest, and Atlassian's platform blocks anything
undeclared. PI Copilot declares none. You can verify this yourself before
installing — see [Verifying these claims](#verifying-these-claims).

## What the app reads from Jira

Read-only. The app requests no write permission of any kind, so Jira refuses a
write even if the code attempted one.

| Read | Used for |
|---|---|
| Issues and their fields | Estimates, story points, assignee, status |
| Worklogs | Hours logged against elapsed capacity |
| Boards and sprints | Which sprints exist and when they run |
| Project versions | Releases shown on the roadmap |
| Users | Names for the roster picker |
| Your Jira admin status | Deciding whether to allow configuration changes |

The app does not read issue descriptions, comments, attachments, or worklog
comments.

## What the app stores, and where

In Atlassian's Forge storage, provisioned inside **your own installation** on
your own site. Not in a database of ours; we do not operate one.

- **People:** Atlassian account ID, display name, email if your Jira privacy
  settings expose it, which working calendar applies, and team membership with
  dates and allocation.
- **Time away:** person, date range, hours per day, and a reason drawn from a
  fixed list — leave, sick, training, support rotation, other.
- **Teams and calendars:** working days, focus factors, public holidays.
- **Planning records:** sprint and PI settings, and snapshots of finished
  sprints and commitments holding issue keys, summaries, estimates, points and
  done state.

### About the sick-leave field

One of the five reasons is `sick`. Stored against a named person with dates,
that is health-related information about an identifiable employee, and some
jurisdictions treat it as a special category requiring a lawful basis and
possibly a DPIA.

We are telling you this rather than burying it, because it is your
responsibility as the data controller, not ours. If your organisation would
rather not record it, use `other` — the capacity arithmetic is identical. The
reason exists to help a team read its own calendar, not to build a health
record.

## Who can see it

- Reads run with the app's own permissions rather than the signed-in user's, so
  a fresh install works without every user granting individual consent. The
  trade-off is real and we state it rather than hide it: **the app's reads are
  not limited by what the person looking is allowed to see in Jira.**
- Every configuration screen — teams, roster, calendars, PIs, commitments — is
  restricted to **Jira administrators**, checked on the server on every call,
  and denied if the check itself fails.
- The one thing a non-administrator can change is their own time away. The app
  takes their identity from Atlassian's signed invocation context and ignores
  any account named in the request, so nobody can file leave against a
  colleague.

## Retention and deletion

Storage is created when you install and deleted when you uninstall. On
uninstall Atlassian soft-deletes the data and disposes of it under its Standard
Data Retention and Disposal policy, covered by Atlassian's SOC 2 report. If you
reinstall within 21 days you can ask Atlassian to relink the previous data;
after that it is gone.

You can also delete teams, people, absences and snapshots from inside the app
at any time.

Because nothing ever leaves your installation, there is no copy held by us to
request, export, or delete. A subject access or erasure request is served
entirely from your own Jira instance.

## What we do not do

No analytics. No telemetry. No crash or error reporting service. No CDN, no
external fonts. No advertising, no profiling, no automated decision-making. No
AI model is called and no data is used to train one. Nothing is sold or shared,
because nothing is received.

## Roles under data protection law

You, the customer, are the **data controller** for the personal data in your
Jira instance and in the app's storage within your installation. Atlassian is
your processor for the infrastructure, under the agreement you already hold
with them.

Onreera does not receive, access, or process that personal data in the ordinary
running of the app. If you ask us for support and choose to send us a
screenshot or an export, that material is handled under the contact section
below and deleted once the question is resolved.

## Verifying these claims

Everything above is a property of the source, not a statement of intent, and
you can check it before you trust it. The app's own verification script fails
if any claim on this page stops being true:

```bash
npm run verify:readonly
```

Or directly:

```bash
grep -n "scopes:" -A 20 manifest.yml     # every permission requested
grep -rn "external" manifest.yml         # egress: there is none
grep -rn "requestJira" src/              # every call made to Jira
```

The install screen Atlassian shows you also lists every permission before you
approve it. If it lists anything beginning with `write:`, this policy is out of
date and you should not install.

## Changes

Material changes will be reflected in the date at the top of this page. A change
that widens what the app reads or stores also requires new permissions, which
Atlassian will hold for your explicit approval — the app keeps running under the
permissions you already granted until you accept.

## Contact

Questions, requests, or a security concern: **info@onreera.com**

---

*This document describes the app's actual behaviour and is not legal advice. If
your organisation requires a Data Processing Agreement or a completed security
questionnaire, contact us at the address above.*
