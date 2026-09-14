# Citing your own incidents

This is the feature that separates a useful assessment from a plausible one.

Before you import anything, Premortem tells you what tends to go wrong with
work like this. After, it tells you what went wrong **to you** — and names the
issue key where it happened.

> "This is how BT-118 started: the nightly job reported a discrepancy nobody
> owned, and four days passed before anyone looked."

Generic risk advice gets skimmed. A sentence citing your own outage does not.

---

## What you need first

**Embeddings.** Finding the incident that resembles the issue in front of you
is a similarity search, and that needs a model that turns text into vectors.

- **OpenAI or Gemini as your chat provider** — already covered, nothing to do
- **Anthropic** — Claude has no embeddings API at all. Set a separate
  embeddings provider under *Settings → Embeddings*: OpenAI, Gemini, or Voyage
- **A custom endpoint** without `/embeddings` — same, choose one explicitly

Import refuses to start without this rather than silently importing something
unsearchable.

## Importing past incidents

**⚙ → Apps → Premortem settings → Your project's incidents.**

Give it a project key (`BT`) and optionally a comma-separated list of issue
types to include. It reads that project's closed bugs and incidents, embeds
them, and stores them in your installation.

- **Re-importing is safe.** Duplicates are skipped
- The page shows how many incidents you currently hold
- Import costs embedding tokens on your key, once per incident

## Which projects to import

Start with the project whose work you are assessing. Add adjacent ones if
teams share systems — an incident in the payments project is relevant to a
reconciliation ticket even though the keys differ.

There is no benefit to importing everything. Precedents from a codebase
nobody in the room has touched produce confident, irrelevant risks.

---

## Postmortems from Confluence

A postmortem is the best precedent there is, because it says *why*: not "the
queue backed up" but "the queue backed up because batch retries shared a
client id with live traffic, and nobody saw it for a week".

**⚙ → Apps → Premortem settings → Your postmortems.** Give it the space keys,
the page label your team uses (default `postmortem`), and/or a parent page,
then press **Import postmortems**.

- **Restricted pages are skipped.** Premortem reads Confluence with its own
  rights, not the viewer's, so a postmortem from a view-restricted page could
  otherwise surface on an issue anyone can open. A page is imported only when
  it and every page above it are verified open; when that cannot be verified,
  the page is skipped
- **Re-importing is safe.** Unchanged pages are skipped, edited ones updated
- Each imported page costs one embedding call on your key
- When a postmortem is relevant, the risk says so: *your postmortem: …*

## What your team adds as it goes

Two buttons in the panel feed the same store, with no import at all:

- **It missed one** — anyone can add a risk the assessment did not raise.
  Later assessments of similar work cite it as *raised by your team before*
- **This happened** — marks a risk that came true. Similar work later cites
  it as *predicted here before, and it happened*

If [outcome matching](settings.md#notice-when-a-risk-comes-true) is on (it is
off by default), Premortem also looks at recently resolved bugs and incidents once a
week and proposes which earlier risk each one was. Nothing is recorded until a
person confirms the match.

---

## When you change embeddings provider

Vectors from different models are not comparable. Rather than compare them
badly, Premortem **skips** incidents embedded with a different provider than
the one now configured, and the settings page warns you with a count.

Re-import the project to bring them back.

## When one incident goes bad

Occasionally a single imported incident keeps producing precedents that are
technically similar and practically useless.

**⚙ → Apps → Premortem settings → Feedback & risk quality** shows what your
team accepts and dismisses, and lets you **suppress** that incident. It stays
imported; it stops being cited. No need to delete and redo the whole import.
