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

## Importing

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
