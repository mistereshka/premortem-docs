# Premortem

Premortem reads a Jira issue — and, if you connect it, the code written for
that issue — and tells you how the work could go wrong before it does.

It is not a place to file risks. It is the thing that finds them, using your
team's own context: the epic, the architecture page, what people said in the
comments, and the incidents you have already lived through. You accept the
ones worth keeping; those go into the issue description and your Confluence
risk register. The rest you dismiss and never see again.

**It runs on your own AI key.** Your text goes from your Atlassian site
straight to the provider you chose — OpenAI, Anthropic, Google, or any
OpenAI-compatible endpoint you point it at. We do not have a server in the
middle, because we do not have a server at all.

---

## Start here

- **[Getting started](getting-started.md)** — install, add a key, first
  assessment. About five minutes.
- **[Settings reference](settings.md)** — every option, what it costs, and
  when to change it.

## Then, when you want more from it

- **[Code risk analysis](code-risks.md)** — connect a Git repository so
  Premortem reviews the actual diff against what the team decided.
- **[The risk register](risk-register.md)** — how accepted risks reach
  Confluence, and how the register is kept from rotting.
- **[Citing your own incidents](incidents.md)** — import past incidents so
  assessments say "this is how PROJ-118 started" instead of speaking in
  generalities.

## Before you decide

- **[Where your data goes](data-and-privacy.md)** — read this one before
  installing, not after.
- **[Troubleshooting](troubleshooting.md)** — the failures people actually hit.

---

## What it needs

| | |
|---|---|
| **An API key** from an LLM provider | Required. Nothing runs without it, and the tokens are billed to you |
| **A Confluence space** | Optional. Only if you want the risk register |
| **A Git repository** | Optional. Only for code risk analysis |
| **Jira admin rights** | To configure it. Anyone who can see an issue can then use it |

There is no account to create with us, and nothing to pay us for at this
stage.
