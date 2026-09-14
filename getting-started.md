# Getting started

Five minutes, and you need to be a Jira administrator for the first two steps.

---

## 1. Get an API key

Premortem has no AI of its own. It calls the provider you choose, with your
key, and the tokens land on your bill. Pick one:

| Provider | Where the key comes from | Sensible first model |
|---|---|---|
| OpenAI | platform.openai.com | `gpt-4.1-mini` |
| Anthropic | console.anthropic.com | `claude-sonnet-5` |
| Google Gemini | aistudio.google.com | `gemini-2.5-flash` |
| Anything OpenAI-compatible | your own endpoint | whatever it serves |

If you have no preference, start with the cheap fast model of whichever
provider you already have an account with. A task assessment is a few
thousand tokens; you are not going to be surprised by the bill on a trial.

## 2. Put the key into Premortem

**Jira → Settings (⚙) → Apps → Premortem settings.**

Under **AI model**, choose the provider, paste the key, and leave the model
field empty unless you want a specific one — empty means the default above.

Press **Save**, then **Test connection**. It calls the provider once and tells
you whether the key works. Do this now rather than finding out from a failed
assessment later.

> The key is stored in Forge's encrypted secret storage, never in plain
> settings, and it is never shown back to you — only the last four
> characters, so you can tell which key is loaded.

## 3. Run the first assessment

Open any Jira issue with a real description. In the right-hand column, expand
the **Premortem** panel and press **Assess risks**.

It takes a few seconds. You will get risks grouped by kind, each with:

- **What could go wrong**, in one sentence
- **A priority** — high, medium or low — with the three ratings behind it:
  severity, how likely it is to occur, and how late you would detect it, each
  from 1 to 5, plus one line on why
- **Mitigation** — what to do so it does not happen
- **If it happens** — what to do when it does anyway
- **Trigger** — the signal that tells you it is happening now

The list keeps the order the assessment wrote it in, with a count of high,
medium and low at the top of each group. The ratings are there to read, not
to sort by: when we measured it, sorting by them pushed the risk that
actually came true further down the list, not up.

Each risk has five buttons: **Accept**, **Mitigating**, **Dismiss**, **Ask
AI**, and **This happened**.

## 4. Decide what to keep

This is the part that matters. The model proposes; you decide. Nothing is
written anywhere until you accept it.

- **Accept** — this is real, keep it
- **Mitigating** — real, and we are already handling it
- **Dismiss** — not relevant. It disappears from the list; **Show dismissed**
  brings it back if you change your mind
- **Ask AI** — argue with it. Useful when a risk is nearly right, or when you
  suspect it misunderstood the issue
- **This happened** — it came true. Later assessments of similar work will
  cite it

If the assessment missed something, write it into the field under the list
and press **It missed one**. It becomes a precedent for similar work too.

Then press **Write accepted**. Two checkboxes decide where the accepted risks
go: the issue description, the Confluence register, or both.

---

## Where to go next

**Read the [settings reference](settings.md) before turning things on.** Most
of the context switches are off by default because each one adds tokens to
every assessment — on your key.

If the assessments feel generic, that is the expected starting state: the app
knows nothing about your systems yet. Two things fix it, in this order:

1. **Point it at your architecture page** — one Confluence link, one minute,
   and it is the single biggest quality jump available. See
   [settings](settings.md#system-architecture).
2. **[Import your past incidents and postmortems](incidents.md)** — so risks
   cite what actually happened to you before, by issue key or by postmortem.
