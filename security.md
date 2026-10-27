# Security

How Premortem is built, what protects your data, and how to report a problem.
This page covers the app's security practices. For a plain-language account of
what leaves Jira and what does not, read [Where your data goes](data-and-privacy.md);
the governing legal text is the [privacy policy](privacy.md).

Last reviewed: 27 October 2026.

---

## Reporting a vulnerability

**security contact: support@usepremortem.com**

Send anything you believe is a security issue there — a monitored mailbox on
our own domain, not a personal address. Please include what you did, what you
saw, and the Atlassian site and app version if you have them.

| | |
|---|---|
| Acknowledgement | within 2 business days |
| Initial assessment | within 5 business days |
| Fix for a confirmed high-severity issue | deployed as a Forge release as soon as it is validated, and reported back to you |

We will not pursue anyone who reports a finding in good faith, and we will
credit you if you want to be credited. Please do not test against other
customers' sites, and do not run load or denial-of-service tests.

Vulnerabilities in the Atlassian platform itself belong to Atlassian's own
[bug bounty programme](https://www.atlassian.com/trust/security/bug-bounty),
not to us.

---

## Architecture: there is no server of ours

Premortem is a **Forge app**. All of its code runs inside Atlassian's own
infrastructure, under the installation on your site. We operate no servers, no
database, no logging pipeline and no network path that receives your content.

That is the single most important security property of this app, and it is
structural rather than promised: there is no system of ours to breach, because
there is none. It also means we cannot read your issues, your code or your
settings — not on request, not under support access, not at all.

The consequences:

- **No shared storage.** Every Atlassian site gets its own Forge installation
  with its own isolated storage. One customer's data is not addressable from
  another's.
- **No employee access to customer data.** There is no console, no admin
  backdoor and no production database to grant access to, so there is no
  access to review, revoke or audit.
- **Platform security is Atlassian's.** Hosting, tenancy isolation,
  patching of the runtime and physical security are Atlassian's
  responsibility under the [Atlassian Trust Center](https://www.atlassian.com/trust),
  and Forge apps inherit it.

## Data handling

### What the app holds

| Data | Where it lives | Deleted |
|---|---|---|
| Settings | Forge storage, your installation only | on uninstall |
| Your AI provider API key | Forge **encrypted secret** storage | when you replace it, or on uninstall |
| Imported incidents and postmortems, and their embedding vectors | Forge storage, your installation only | on uninstall |
| Risks recorded by your team, including the Atlassian account ID of whoever recorded one | Forge storage, your installation only | on uninstall |
| Who accepted or dismissed a risk, including their Atlassian account ID | a property on that Jira issue, inside your site | with the issue |

Issue text, page text and code diffs are **processed and never stored** by the
app. They are read, sent to the AI provider you configured, and dropped.

### Encryption

- **In transit:** every outbound request is HTTPS/TLS. There is no plaintext
  transport anywhere in the app, and no option to disable it.
- **At rest:** Forge storage is encrypted at rest by Atlassian.
- **Secrets:** your provider API key is written to Forge's encrypted secret
  storage, which is write-only to the app — it is sent in the `Authorization`
  header of the request to your provider and is never returned to the user
  interface, never written to an issue, and never logged.

### Where data is allowed to go

The app's manifest declares an **egress allowlist**, enforced by the Forge
platform rather than by our code. Outbound traffic is only possible to these
four hosts:

- `api.openai.com`
- `api.anthropic.com`
- `generativelanguage.googleapis.com`
- `api.voyageai.com` — optional, only if an administrator chooses Voyage for
  embeddings

The app cannot reach anything else, including any host of ours. Which of those
hosts is used, and whether any is used at all, is the administrator's choice,
authenticated with the customer's own key. That provider is a data processor
under **your** agreement with them.

You can verify the allowlist yourself: it is the `permissions.external.fetch`
section of the app's manifest, which Atlassian validates on every deployment.

### Least-privilege scopes

The app requests only the scopes it uses, and Atlassian shows them to an
administrator before installation:

`read:jira-work`, `write:jira-work`, `search:confluence`,
`read:space:confluence`, `read:page:confluence`,
`read:content-details:confluence`, `write:page:confluence`, `storage:app`.

There is no user-impersonation scope, no administration scope and no scope for
reading Jira or Confluence configuration beyond the above.

### Access control inside the app

- Everything that changes configuration or spends money — the settings page,
  connection tests, incident and postmortem imports — **verifies that the
  caller is a Jira administrator** on the server side, in the resolver, not
  only by hiding a button.
- **Restricted Confluence pages are never imported.** The app reads Confluence
  with its own rights, so before importing a page it checks that the page and
  every page above it are open to view; a restricted page is skipped. This
  exists specifically so the app cannot be used to surface content past a
  restriction.
- The optional CI integration uses a Forge **web trigger**. Its URL is itself
  a credential, and the app can regenerate it, and rotate the signing secret,
  from the settings page at any time without redeploying.

## Secure development

- **Automated tests** run against the full resolver surface before any
  release, including the permission and licence checks described above (582
  tests at the time of writing).
- **Static checks**: `forge lint` and ESLint must pass; the Forge platform
  independently rejects a deployment whose manifest violates its rules.
- **Dependency hygiene**: the app ships a small, pinned dependency set, which
  is reviewed and updated on each release. The Forge runtime itself is patched
  by Atlassian.
- **Release path**: code reaches customers only through
  `forge deploy` to the production environment, which Atlassian signs and
  distributes. There is no other way to ship code into the app, and no
  mechanism for us to push code to one customer and not another.
- **No telemetry.** The app sends no analytics, crash reports or usage data
  anywhere.

## Incident response

If we become aware of a security incident affecting the app, we will
investigate immediately, publish a fix as a Forge release, and notify affected
Marketplace customers by email at the technical contact on their installation,
with what happened, what data was involved and what to do — within 72 hours of
confirming it, and sooner if the information is available sooner.

Because the app holds no copy of your data outside your own Atlassian site,
there is no dataset of ours that an incident could expose.

## Sub-processors

| Who | What for |
|---|---|
| **Atlassian** | hosting and execution of the app, and all storage of app data, inside your site's own Forge installation |
| **The AI provider you configure** — OpenAI, Anthropic, Google, or Voyage for embeddings | processing the text you send for assessment, under your own agreement and your own key |

There are no others. We do not use a CDN, an analytics provider, an error
tracker or a hosting provider of our own, because there is nothing of ours to
host.

## Certifications

**Premortem holds no independent security certification.** We are not SOC 2 or
ISO 27001 audited, and we will not claim to be. We would rather tell you that
than imply otherwise.

What we can point to instead is that the app stores no customer data outside
the customer's own Atlassian site, and that the platform it runs on —
Atlassian Cloud — is itself SOC 2 Type II and ISO/IEC 27001 certified, which
is where your data actually lives. Atlassian publishes its reports at the
[Atlassian Trust Center](https://www.atlassian.com/trust/compliance).

## Data deletion

Uninstalling the app removes its Forge storage, and with it the settings, the
API key, the imported incidents and postmortems and their vectors, and the
recorded risks.

Two things survive on purpose, because they are yours and are inside your
site: risks already written into Jira issue descriptions or into your
Confluence risk register, and the per-issue property recording who accepted or
dismissed a risk. Delete the issue or the page and they go with it. If you want
them cleared while keeping the issues, write to
support@usepremortem.com and we will tell you exactly which property to
remove — we cannot do it for you, because we have no access to your site.

---

Questions about anything on this page: **support@usepremortem.com**.
