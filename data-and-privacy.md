# Where your data goes

Read this before installing, not after. It is short because the architecture
is simple.

---

## We have no server

Premortem runs entirely on Atlassian Forge. There is no backend of ours
anywhere in the path — no database, no logs, no infrastructure that receives
your content. We could not read your issues if we wanted to.

This is not a policy promise. It is what the app is made of.

## Exactly one third party sees your text, and you chose it

When an assessment runs, the issue text — plus whichever context sources you
enabled — goes **from Atlassian's infrastructure straight to the AI provider
you configured**, authenticated with your key.

That provider is a data processor under **your** agreement with them, not
ours. Before enabling the app, check their terms on retention and training,
because we have no visibility into either.

## What leaves Jira, and what does not

**Always sent** to your provider when you run an assessment:

- the issue's summary and description
- the diff, for a code assessment

**Sent only if you switched it on** — every one has its own toggle under
*Context depth*:

- the parent epic, other active issues, the architecture page
- for a code assessment: the changed-file names, the pull request's title and
  description, and the call sites of the changed code — these three are on by
  default, and your CI is what collects them
- your own history, when it resembles the issue: imported incidents,
  postmortems imported from Confluence, risks your team added by hand, and
  risks that came true
- **comments, including commenter display names** — off by default
- linked issues, labels and components
- full file contents, commit history **including commit author names**, code
  from related issues — all off by default

**Outcome matching** is off by default too. When on, recently resolved bugs
and incidents are compared with risks raised earlier, once a week, and the
text of both goes to your provider.

A switched-off source is not read at all. No API call is made for it, so the
data never leaves Jira in the first place.

**Postmortems from restricted pages are never imported.** Premortem reads
Confluence with its own rights, so a page is imported only when it and every
page above it are verified open to view.

**Never sent anywhere:** your API key goes only in the `Authorization` header
to your own provider. Risk ratings and who made them stay inside your site.

## What is stored, and where

| What | Where | Lives until |
|---|---|---|
| Settings | Forge storage, your installation only | you uninstall |
| Your API key | Forge **encrypted secret** storage | you uninstall or replace it |
| Imported incidents and postmortems, and their vectors | Forge storage, your installation only | you uninstall |
| Risks your team added, and what came true — **including the Atlassian account ID of whoever recorded it** | Forge storage, your installation only | you uninstall |
| Who accepted or dismissed a risk — **including their Atlassian account ID** | a property on that Jira issue, inside your site | the issue does |
| Risks you wrote out with **Write accepted** | your Jira descriptions and Confluence pages | you delete them |

Those account IDs are why the app answers **yes** to Atlassian's *Stores
personal data?* question. They never reach the AI provider and never reach
us; they exist so "who dismissed this, and when" has an answer.

## Cost, since it is the other thing people ask

Tokens are billed to you by your provider at your rate. The app cannot show
you money — it does not know your rate — so it shows **tokens** under each
assessment and lets an admin cap **runs per month**. See
[the monthly limit](settings.md#monthly-limit).

## Isolation between sites

Every Atlassian site gets its own Forge installation with its own storage.
One customer's incidents, settings and keys are not reachable from another's.

---

The full legal text — privacy policy and EULA — is published separately and
is the document that governs. This page exists so you can understand the
shape of it in two minutes.
