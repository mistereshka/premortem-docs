# How to run a pre-mortem on a Jira issue

A pre-mortem is the opposite of a post-mortem: you assume the work has already
failed and ask why, before anyone writes code. The technique comes from Gary
Klein, and it works because imagining a failure that has already happened is
much easier for people than rating hypothetical probabilities.

This page is how to do it on a Jira issue. The manual version takes fifteen
minutes and needs no tooling; the last section is what our app does with it.

---

## The manual version, in fifteen minutes

Do it at the point where the approach is decided but no code exists —
grooming, or the first day of the ticket.

**1. State the failure as fact.** Not "what could go wrong", which gets you
platitudes. Say: *"It is three weeks from now. This shipped and it went
badly. What happened?"*

**2. Everyone writes alone for two minutes.** Silent and independent, before
anyone speaks. The first answer said out loud anchors the room, and the
quietest person in the room is usually the one who has seen this fail before.

**3. Read them out, one each, round the table** until the lists are empty. No
discussion while reading — discussion is what makes people drop their weaker
item, and the weaker item is often the real one.

**4. For each risk that survives, write three things.** A risk with none of
these is a worry, not a risk:

| | |
|---|---|
| **Mitigation** | What we do now so it does not happen |
| **Contingency** | What we do if it happens anyway |
| **Trigger** | The observable signal that it is happening — a log line, a metric, a support theme |

**5. Put them in the issue, not in the meeting notes.** A risk in a document
nobody opens again has not been recorded. Three lines at the bottom of the
issue description is enough, and it is where the person doing the work will
actually see it.

## The four questions that produce most of the real risks

If the room is quiet, these work better than a generic prompt:

- **What does the ticket not say?** The unwritten assumption is where most
  failures live: that the data is clean, that the third party is reliable,
  that nobody is doing this twice at once.
- **What else touches this?** Not the code we are changing — the thing
  downstream that reads it, the job that runs at midnight, the report somebody
  built on top.
- **What happens if it runs twice?** Retries, double clicks, a webhook
  delivered again. Idempotency is the most common thing to be missing and the
  cheapest to add before the code exists.
- **Which of our own past incidents does this rhyme with?** This is the
  question that produces the best risks and the one teams skip, because
  remembering requires someone who was there.

## Why most pre-mortems stop after three sprints

They are a meeting, and meetings lose to delivery pressure. The two failures
we see most:

- **The output is generic.** "The API might be slow." True of everything, acts
  on nothing. The fix is the trigger column: a risk you cannot name a signal
  for is not specific enough yet.
- **Nobody reads the risks afterwards.** A register that is written and never
  revisited is a cost with no return. The habit that keeps it alive is small:
  when something does go wrong, go back and mark which risk predicted it, and
  when a risk is closed out, say so.

## Where Premortem fits

[Premortem](index.md) does step 1 to 4 on an issue in a few seconds, so the
fifteen-minute meeting becomes a five-minute review of a list someone else
drafted. It reads the issue, its epic, the decisions in the comments and your
architecture page, and it names risks with the mitigation, contingency and
trigger already filled in. You accept the ones that are real; those go into
the issue description and, if you want, a
[Confluence risk register](risk-register-in-confluence.md). The rest
disappear.

The part it does that a meeting usually cannot: if you
[import your closed incidents](incidents.md), the risks cite them by key —
"this is how PAY-482 started" — which is the fourth question above, answered
without needing the person who was there.

It is honest about what it is: we [published a measurement](https://github.com/mistereshka/premortem-eval)
of it against six open-source changes whose later regressions are recorded in
public trackers. Two hits, two near misses, two misses. It is a drafting tool
for the people in the room, not a replacement for them.

- [Getting started](getting-started.md) — install, key, first assessment, five
  minutes
- [Where your data goes](data-and-privacy.md) — it runs on your own AI key and
  we operate no server
