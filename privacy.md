# Privacy Policy

**Premortem — Risk Assessment for Jira**

Last updated: 21 September 2026

This policy describes how the Premortem app ("the App"), published by
Premortem ("we", "us"), handles data. It is written from the App's actual
architecture, and every claim in it can be checked against the app manifest
that Atlassian publishes for the App.

## The short version

**We operate no servers and we receive none of your data.** The App runs
entirely inside Atlassian's Forge platform. We have no backend, no database,
and no logs that see your content. The only place your text goes is the AI
provider *you* choose and authenticate with *your own* key.

## What the App does

The App assesses the technical risk of a Jira issue, and of code changes made
for it, using an AI language model you select and pay for directly. It can
also use your organisation's own history as context: past incidents,
postmortems, risks your team added by hand, and risks that were predicted and
later came true.

## What data the App reads, and where it goes

| Data | Why | Where it goes |
|---|---|---|
| Issue summary and description | The subject of the assessment | Your chosen AI provider only |
| Code diffs sent by your CI | Same, for code assessment | Your chosen AI provider only |
| With a diff: the changed-file names, the pull request's title and description, and the call sites of the changed code | Shows what a change can break outside the diff | Your chosen AI provider only. Collected by your CI workflow; each can be switched off |
| Confluence architecture page, if you link one | Grounds advice in your real components | Your chosen AI provider only |
| Parent epic; titles of other open issues | Places the work in context | Your chosen AI provider only |
| Your imported past incidents | Lets risks cite real precedent | Your provider's embeddings endpoint; stored in Forge storage; the most similar ones are sent to your chosen AI provider with an assessment |
| Confluence postmortem pages an administrator imports | Lets risks cite what your team learned | Same as imported incidents. A page that is restricted from view, or sits under a restricted page, is skipped, and so is any page whose restrictions cannot be checked |
| Risks your team adds by hand, and risks marked as having happened | Become precedents for later assessments | Same as imported incidents |
| Your AI provider API key | To call that provider on your behalf | Forge encrypted secret storage; sent only in the `Authorization` header to that provider |

The following are **switched off by default** and read only if an
administrator enables them. A source that is off is not read at all — no API
call is made for it, so the data never leaves Jira:

- Comments on the issue, **including commenter display names**
- Linked issues and subtasks; labels, components and priority
- Full contents of changed files
- Commit history of changed files, **including commit author names**
- Code stored for related issues
- A standing instruction written by your administrator
- **Outcome matching.** Once a week, or when an administrator presses *Look
  now*, recently resolved issues of the types the administrator lists are
  compared with risks raised earlier. The text of each such issue, and the
  risks it is compared with, are sent to your chosen AI provider

**None of the above ever reaches a server operated by us**, because no such
server exists.

## Personal data

The App handles personal data in the following ways.

**Atlassian account IDs.** When somebody accepts, dismisses or rates a
suggested risk, the App records that action together with the account ID of
the person who took it and a timestamp. This is written to a property on that
Jira issue — inside your own Atlassian site. When somebody marks a risk as
having happened, confirms a proposed match, or adds a risk the assessment
missed, the account ID is recorded with it in Forge storage for your
installation. Account IDs are never sent to the AI provider and never sent to
us. They exist so that "who dismissed this, and when" has an answer.

**Names in text sent to the AI provider.** If your administrator enables the
comments or commit-history context sources, the display names of commenters
and commit authors are included in the text sent to your AI provider. Both
sources are off by default. Incidents and postmortems often name people too:
whatever names an imported issue or page contains are part of the text that is
embedded and, when relevant, sent to your provider.

## The AI provider is your processor, not ours

When you configure a provider — OpenAI, Anthropic, Google, or a compatible
endpoint — that provider processes the content sent to it under **your**
agreement with them. We are not party to it, have no visibility into it, and
do not control it.

**Review your provider's own terms before enabling the App**, in particular
whether they retain API traffic or train on it.

## How the App is secured

- **There is no infrastructure of ours to breach.** The App is Forge code and
  Forge storage inside Atlassian's platform. We run no servers, no database
  and no logs, and we hold no standing access to any customer's site.
- **Encryption.** Your AI provider key and your CI signing secret are held in
  Forge's encrypted secret storage, never in plain settings, and neither is
  ever displayed back — the key shows its last four characters only. Every
  call the App makes, to Atlassian and to your providers, is over TLS.
- **Isolation.** Every Atlassian site has its own Forge installation and its
  own storage. One customer's settings, keys and imported history are not
  reachable from another's.
- **Configuration is administrator-only.** Reading or changing settings —
  including the key, the imported incidents and the CI secrets — requires
  Jira administrator permissions. Anyone who can see an issue can run an
  assessment on it; only an administrator configures the site.
- **The CI endpoint is signed.** The web trigger your pipeline posts diffs to
  accepts only requests carrying a correct HMAC-SHA256 signature made with a
  secret created for your site, compared in constant time. Unsigned or
  wrongly signed requests are rejected and nothing is stored. Both the URL and
  the secret can be rotated from the settings page at any time.
- **Confluence is read with the App's own rights, and that is accounted for.**
  A postmortem page is imported only when it, and every page above it, is
  verified open to view; a page whose restrictions cannot be checked is
  skipped rather than imported.

## Storage, retention and deletion

- Settings, your API key, imported incidents and postmortems, risks your team
  added, outcome records and feedback records are held in Forge storage,
  isolated to your installation, for as long as the App is installed. Outcome
  records stay when the issue they came from is deleted, because what they
  hold — what was predicted, and whether it happened — does not depend on the
  issue still existing.
- Uninstalling the App triggers Atlassian's standard Forge app-data lifecycle,
  which removes the App's stored data for your installation.
- Risks you choose to write into a Jira description or a Confluence page
  become ordinary Jira and Confluence content, governed by your own site's
  retention settings rather than by the App.

Every Atlassian site has its own installation and its own storage. One
customer's data is not reachable from another's.

## What we ask of you

- Keep your own AI provider account secure; the App uses the key you give it.
- Decide, as the data controller for your site, whether sending your issue
  text to your chosen provider is appropriate for your organisation.
- If you enable the comment or commit-history context sources, be aware that
  you are sending colleagues' names to that provider.
- Import postmortems from pages you would be comfortable citing on any issue:
  the restriction check keeps restricted pages out, but it cannot judge a page
  that is open yet sensitive.

## Children

The App is a workplace tool and is not directed at children.

## Changes

We will update this page when the App's behaviour changes, and the date at
the top will change with it.

## Contact

Questions about this policy, or about data the App handles: alexeyshipin2@gmail.com
