# PI Copilot — Privacy Policy

**Last updated:** 5 November 2026
**Published by:** Onreera
**Contact:** info@onreera.com

PI Copilot is a capacity and velocity planner for SAFe Program Increments, built
as an Atlassian Forge app for Jira Cloud.

This policy exists because the app handles information about named employees —
including when they are off sick. It is written to be read by the administrator
who has to approve the install, not to be skimmed.

## The short version

**Your Jira data does not reach Onreera.** The app runs entirely inside Atlassian's
infrastructure and cannot make a network call to anywhere else. There is no
server of ours for your data to reach, and the app writes nothing to the logs
Atlassian shows its developer. Failures are kept in your own installation, described
under [Diagnostics](#diagnostics), and reach us only if you choose to send them.

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

## Diagnostics

- **A failure log in your installation.** When a call to Jira fails, the app records
  the time, the build, the Jira path with identifiers removed, the status, Jira's
  request id, the response headers that identify a failure, and the error page or
  message reduced to its text, with emails and account ids removed. It keeps at most
  200 lines, until they are overwritten or the app is uninstalled. A site where nothing
  fails writes nothing.
- **Who can read that log.** A Jira administrator gets it in full. Anyone else who can
  open the app gets it shortened to when, which call, the status and the request id,
  without what Jira said to other people. If Jira cannot confirm who is an administrator,
  everyone gets the shortened copy.
- **A file you choose to save.** *Download diagnostics* saves a report to your own
  computer. It goes nowhere unless you send it. An error page in it is reduced to its
  visible text, which could print a name, so look at it before forwarding it.
- **Nothing from the app to us.** The app writes nothing to the logs Atlassian shows its
  developer, and the check under [Verifying these claims](#verifying-these-claims) fails
  if its code starts to. Atlassian's platform can record errors of its own, such as a
  run timing out, and its developer console shows us which sites have run the app in
  the last 14 days. Those are Atlassian's records rather than something the app writes.

## Who can see it

- Every Jira read that fetches data runs **as you**, with your own permissions.
  Jira itself limits what the app can read to what you can already see. Atlassian
  may ask you once to allow this. The app's own identity is used only for health
  checks, which keep a status and not the data.
- On top of what Jira enforces, the app decides per team what you may see and
  change, on every request, before reading anything.
- A team is bound to a Jira board, and access follows the viewer's permissions
  on that board's project, checked on the server each time:
  - **Browse** the project → see that team's planning, capacity and calendar.
  - **Administer** the project (or be a global Jira administrator) → change
    that team: its roster, PIs, commitments, and other people's time away.
  - Neither → the team does not appear anywhere, and a request for it by id is
    refused.
- A person's record is shown to an administrator only with the memberships on
  teams they may see. Their time off — including sick leave — is visible only
  to people who may see a team they are on.
- The one thing everyone can change is their own time away. The app takes
  their identity from Atlassian's signed invocation context and ignores any
  account named in the request, so nobody can file leave against a colleague
  they do not administer.
- If the permission check itself fails, nobody is granted anything.

## Retention and deletion

Storage is created when you install and deleted when you uninstall. On
uninstall Atlassian soft-deletes the data and disposes of it under its Standard
Data Retention and Disposal policy, covered by Atlassian's SOC 2 report. If you
reinstall within 21 days you can ask Atlassian to relink the previous data;
after that it is gone.

You can also delete teams, people, absences and snapshots from inside the app
at any time.

The app sends nothing out of your installation, so we hold no copy to request,
export, or delete unless you sent us something for support. A subject access or
erasure request is served from your own Jira instance, plus any diagnostics file
your people saved or sent.

## What we do not do

No analytics. No telemetry. No crash or error reporting service. No CDN, no
external fonts. No advertising, no profiling, no automated decision-making. No
AI model is called and no data is used to train one. Nothing is sold or shared.
The app sends us nothing, and anything you send us for support is used only to
answer that question.

## Roles under data protection law

You, the customer, are the **data controller** for the personal data in your
Jira instance and in the app's storage within your installation. Atlassian is
your processor for the infrastructure, under the agreement you already hold
with them.

Onreera does not receive, access, or process that personal data in the ordinary
running of the app. If you ask us for support and choose to send us a
screenshot, an export or a diagnostics file, that material is handled under the contact section
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
