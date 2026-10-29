# Keeping a risk register in Confluence without typing it twice

Most risk registers die the same way. Someone creates a Confluence page with a
good table, fills it in for two sprints, and then stops — because every row
has to be typed by hand, in a second place, about work that is already
described in Jira. Six months later the page is a record of what the team
worried about in March.

This page is how to keep one that stays current, whether or not you use our
app.

---

## What a register row has to contain to be worth keeping

A table of risk titles is a list of worries. Five columns make it a register:

| Column | Why it is there |
|---|---|
| **The risk, in one sentence** | Written as a thing that happens, not a topic. "The import runs twice and duplicates invoices", not "import reliability" |
| **Where it came from** | The Jira issue key. Without it, nobody can tell six weeks later what work the risk was about |
| **Priority** | Enough to sort the page. Severity and likelihood are worth recording; see the warning below about sorting by them |
| **Mitigation** | What is being done now. This is the column auditors read |
| **Contingency and trigger** | What we do if it happens anyway, and the signal that it is happening. A risk with no trigger cannot be watched |
| **Status** | Open, mitigating, closed, or it happened. A register with no closed rows is not being maintained |

One warning from our own measurement: we had the model rate every risk on
severity, occurrence and detection and sorted the list by the resulting
priority score. Measured against tickets whose real failure was known, the
risk that actually came true ranked **worse** after sorting than it did in the
original order, every time. Record the ratings — they are useful to read — but
be careful about ordering a register by a computed score.

## Why the page rots, and the three habits that stop it

- **It is a second place to type.** If recording a risk means leaving the
  issue, it will not happen on a busy sprint. Keep the authoritative text in
  the issue and let the register be generated from it, in whichever direction
  your tooling allows.
- **Nothing ever closes.** Reserve ten minutes at the end of a sprint to mark
  rows closed and to mark the ones that came true. A register where everything
  is still Open tells a reader that nobody is looking at it.
- **Deleted work leaves orphan rows.** Issues get deleted and moved. Whatever
  generates the page has to notice, or the register slowly fills with rows
  pointing at nothing.

## The simplest manual version

If you want one today with no tooling: a single Confluence page per project
with the table above, a row added at the end of grooming, and the issue key in
every row. Put the page in the project's space, not in a personal one, and
link it from the project overview. That is already better than most registers
we have seen, and it survives exactly as long as someone keeps adding rows.

## The generated version

[Premortem](index.md) writes the register for you, from decisions you have
already made in Jira. The flow has one deliberate rule: **nothing is written
until you accept it.**

1. Open an issue and run an assessment. The app proposes risks, each with
   mitigation, contingency and trigger already written.
2. Accept the ones that are real. Dismiss the rest; they disappear.
3. Press **Write accepted**. The risks go into the issue description and, if
   the register is switched on, into a Confluence page — one row per accepted
   risk, with the issue key, the priority, the mitigation and the trigger.
4. Mark a risk **This happened** when it comes true. The register says so, and
   later assessments of similar work cite it.

Two details that decide whether a generated register stays usable: writing the
same issue twice updates its rows instead of appending a second copy, and
issues that no longer exist are cleaned out of the page rather than left as
orphans.

See [the risk register page](risk-register.md) for the space setup and
exactly what gets written, and
[how to run a pre-mortem on a Jira issue](premortem-on-a-jira-issue.md) for
where the rows come from in the first place.

- [Getting started](getting-started.md) — five minutes, and you need a Jira
  admin for the first two steps
- [Where your data goes](data-and-privacy.md) — your text goes from Atlassian
  straight to the AI provider you chose, on your key
