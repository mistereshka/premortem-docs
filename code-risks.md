# Code risk analysis

The other half of Premortem. Instead of assessing what the issue *says*, it
assesses what was actually written for it — and, when you have pointed it at
an architecture page, whether the code contradicts what the team wrote down.

This is the part no risk register can do.

---

## How it works

1. Someone opens a pull request on a branch named after a Jira issue
2. A GitHub Action sends the diff and PR context to your Premortem
   installation, which **stores it against that issue**
3. Nothing runs yet. No AI call, no cost
4. A person opens the Jira issue and clicks **Assess code risks**
5. The assessment runs on your key and appears in the **Code risks** tab

Step 3 is deliberate. CI fires on every push; assessments cost money. So CI
only ever parks the diff, and a human decides when it is worth assessing.

---

## Setting it up

### 1. Get the two secrets from Jira

**⚙ → Apps → Premortem settings → CI integration.**

You need a Jira admin, but you do not need Forge CLI or developer access.
The page gives you:

- `PREMORTEM_WEBTRIGGER_URL` — where CI posts. Treat it as a secret: it
  contains an unpredictable path
- `PREMORTEM_CI_SECRET` — signs the payload. Created for you the first time
  you fetch the URL, and shown on the same screen

**Unsigned requests are rejected.** Without the secret, nothing is assessed.

### 2. Add them to GitHub

For one repository: *Settings → Secrets and variables → Actions*.

**For several repositories, do not paste the secret into each one.** Use an
organization secret — *org Settings → Secrets and variables → Actions → New
organization secret* — scoped to all or selected repositories. Set once,
every repository in scope sees it.

By CLI:

```bash
gh secret set PREMORTEM_WEBTRIGGER_URL --org <org> --visibility selected --repos repo1,repo2 --body "<url>"
```

That is your own action with your own GitHub credentials. Premortem never
sees or touches your GitHub organisation.

### 3. Add the workflow

Copy the workflow from the same settings screen into
`.github/workflows/premortem.yml`.

### 4. Name branches after issues

The issue key is taken from the branch name (`BT-12-fix-webhook`) or the PR
title (`[BT-12] ...`). No key, no assessment — the Action simply does nothing.

---

## What gets sent

Always: the diff against the PR's base branch, the changed-file list, the
full current content of each changed file (capped), and the PR title and
description.

Whether the **full file content is actually used** is a separate, per-site
decision under *Context depth* — off by default, because it noticeably raises
the cost of each assessment. Sending it and not using it costs nothing; it
means the option is there the day you want it.

---

## Rotating and revoking

- **Rotate CI secret** — invalidates the old signature. Update the GitHub
  secret afterwards or the pipeline starts failing signature checks
- **Regenerate CI webhook URL** — invalidates the URL itself. Use this after
  giving someone temporary access

With an organization secret, either is one update, not one edit per
repository.

To give one person test access without exposing everything: make a separate
org secret scoped only to their repository, or hand them the URL and
regenerate it afterwards.

---

## When it does not fire

| Symptom | Cause |
|---|---|
| Nothing appears in the panel | No issue key in the branch name or PR title |
| Pipeline logs a 401 | Wrong or missing `PREMORTEM_CI_SECRET` |
| "No CI signing secret" in the response | The site has never fetched the webhook URL — do step 1 |
| Diff arrives but nothing is assessed | Working as designed. Click **Assess code risks** on the issue |

More in [troubleshooting](troubleshooting.md).
