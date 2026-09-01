# Settings reference

**Jira → Settings (⚙) → Apps → Premortem settings.** Administrators only.

Everything here is per Atlassian site. There is no per-project or per-user
configuration.

---

## AI model

| Field | What it does |
|---|---|
| **Provider** | OpenAI, Anthropic, Google Gemini, or a custom OpenAI-compatible endpoint. Nothing runs until one is chosen |
| **API key** | Yours. Stored encrypted, never displayed back — you see the last four characters only |
| **Model** | Leave empty for the provider's default. Set it to pin a specific model |
| **Base URL** | Only for the custom provider. Must speak the OpenAI chat-completions shape |

Defaults per provider: `gpt-4.1-mini`, `claude-sonnet-5`,
`gemini-2.5-flash`.

**Test connection** makes one real call and reports what came back. Use it
after every key change — a wrong key otherwise surfaces as a failed
assessment several minutes later.

### A note on cost

You are billed by your provider, per token, at your own rate. Premortem
cannot show you money because it does not know your rate — a negotiated
price, a different model, a provider we have no price list for. What it
shows instead is **tokens used**, printed under each assessment, and what it
caps is **runs per month**.

---

## Embeddings

Only relevant once you [import incidents](incidents.md). Embeddings are how
Premortem finds the incident that resembles the issue in front of it.

Leave it alone and it reuses your chat provider's key. Set it explicitly if
your chat provider has no embeddings API — **Anthropic does not**, so an
Anthropic site must choose OpenAI, Gemini, or Voyage here.

> If you change the embeddings provider after importing, the old incidents
> become unsearchable — vectors from different models are not comparable, so
> they are skipped rather than compared badly. The settings page warns you
> with a count, and re-importing fixes it.

---

## System architecture

One Confluence link. **This is the highest-value field on the page.**

With it, mitigations name your actual components — "make PayGateway refuse
requests when identity claims are ambiguous" — instead of giving generic
advice that applies to any system anywhere. Without it, the model is guessing
at your architecture from the issue text.

Accepts a page URL or a bare page ID.

---

## Confluence space for the risk register

Where accepted risks are written. Two forms:

- `MFS` — one space for every Jira project
- `BT:MFS, PAY:FIN` — per-project mapping, comma or newline separated. An
  entry without a colon is the fallback for anything unlisted

See [the risk register](risk-register.md).

---

## Monthly limit

Anyone who can see an issue can start an assessment, and every one spends
your API key. This caps how many AI runs the whole site may make per calendar
month — task assessments, code assessments and Ask AI messages all count
against the same number.

- **Empty means no limit.** That is the default, and an upgrade never starts
  refusing work on its own
- The cap **applies to everyone, administrators included**. Nothing on the
  settings page is behind it, so if you run into your own limit you raise it
  here and carry on
- A refused run does not consume allowance, so a site at its ceiling recovers
  the moment you raise the number
- The allowance resets on the 1st, UTC
- **Check usage this month** shows where you stand

A garbled value reads as "no limit" rather than as zero — a typo must not take
the site offline.

---

## Context depth

What each assessment is allowed to read. **The two sides are independent**: a
source enabled for task risks is not used for code risks.

Every source costs tokens on your key. The ones that cost noticeably more are
marked in the UI.

### Task risk context

Always included: the issue's summary and description.

| Source | Default | Worth knowing |
|---|---|---|
| Parent epic | on | Cheap, and usually where the real intent lives |
| Other active tasks in the project | on | Catches collisions with work in flight |
| Architecture page from Confluence | on | Does nothing until you set the page above |
| Your past incidents | on | Does nothing until you import them |
| Comments on the issue | **off** | Expensive, and the best single source there is. Decisions get made in comments |
| Linked issues and subtasks | **off** | |
| Labels, components, priority | **off** | Cheap. Useful if your team encodes meaning in them |
| Code already pushed for related issues | **off** | Expensive |

### Code risk context

Always included: the diff itself.

| Source | Default | Worth knowing |
|---|---|---|
| Names of the changed files | on | |
| Pull request title and description | on | |
| Architecture page from Confluence | on | This is what lets it catch code contradicting a written decision |
| Your past incidents | on | |
| Full content of the changed files | **off** | Expensive. Turn on when diffs alone leave too little context |
| Commit history of the changed files, and matching tests | **off** | |
| Code already pushed for related issues | **off** | Expensive |

The last two need an up-to-date CI workflow. If yours predates them, copy the
current one from the CI section of the settings page.

### Standing instructions

Free text, up to 2000 characters, one per side, each with its own on switch.
Written into every prompt on that side.

Use it for things that are permanently true about your context and that the
model cannot infer:

> We are under GDPR — always consider personal-data handling.

> All service-to-service calls here are gRPC — don't suggest REST.

The text and the switch are separate on purpose: you can draft and refine an
instruction while it stays out of the prompt, and turn it on when it is
right.

---

## Your project's incidents

See [citing your own incidents](incidents.md).

---

## CI integration

See [code risk analysis](code-risks.md).

---

## Register maintenance

Removes rows from the Confluence register whose Jira issue no longer exists
or is no longer relevant. Run it occasionally; a register nobody trims stops
being read. See [the risk register](risk-register.md#keeping-it-from-rotting).

---

## Feedback & risk quality

Shows what your team has been accepting and dismissing, and lets you suppress
an incident that keeps producing bad precedents. If one imported incident is
poisoning assessments, this is where you switch it off without deleting your
import.
