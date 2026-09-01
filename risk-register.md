# The risk register

Where accepted risks live once a human has agreed they are real.

Premortem is not primarily a register — it is what fills one. But a risk
nobody can find later is a risk nobody acts on, so accepted risks get written
somewhere durable.

---

## The flow

**Propose → decide → commit.** Nothing is written until the third step.

1. The assessment **proposes** risks in the panel. They exist only there
2. A person marks each **accepted**, **mitigating**, or **dismissed**
3. **Commit accepted** writes them out

Two checkboxes control where:

- **to the issue description** — a "Risks (Premortem)" section, so anyone
  reading the ticket sees them without opening anything
- **to the project's Confluence register** — the cross-project view

Use both. They answer different questions: the description answers "what
should I worry about in this ticket", the register answers "what are we
carrying across the project".

---

## Setting up the Confluence side

**⚙ → Apps → Premortem settings → Confluence space(s) for the risk register.**

- `MFS` — every Jira project writes into that one space
- `BT:MFS, PAY:FIN` — per-project mapping, comma or newline separated. An
  entry with no colon is the fallback for any project not listed

Premortem creates a page called **Risk register BT** (the project key) in the
mapped space if it does not exist yet.

## What a row contains

| Column | |
|---|---|
| Task | linked back to the Jira issue |
| Risk | what could go wrong |
| Category | security, data integrity, operational, and so on |
| Likelihood × Impact | how the panel ordered it |
| Mitigation (prevent) | what stops it happening |
| If it happens | what to do when it happens anyway |
| Trigger | the signal that says it is happening now |
| Status | proposed, accepted, mitigating, dismissed |

The last three columns are the ones that make a register usable in an
incident rather than only in a planning meeting.

## Re-committing is safe

Rows for an issue are **replaced**, not appended. Commit the same issue five
times and the register holds one current set, not five copies. The same is
true of the description section — it is rewritten in place, not stacked.

---

## Keeping it from rotting

A register nobody trims stops being read, and then stops being written.

**⚙ → Apps → Premortem settings → Register maintenance** scans every page
titled `Risk register *` across the site, removes rows whose Jira issue no
longer exists, and saves a page only when something actually changed.

It also runs on a schedule, so in practice you rarely need the button. It is
there for when you have just deleted a batch of issues and want the register
correct now rather than tomorrow.
