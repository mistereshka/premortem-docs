# Troubleshooting

---

## The panel says the assessment won't run

**Provider not selected, or no key.** ⚙ → Apps → Premortem settings → AI
model. Choose a provider, paste a key, **Save**, then **Test connection**.

Test connection is the fast way to tell a bad key from everything else: it
makes one real call and reports exactly what the provider said.

## "This site has used its monthly limit"

An administrator capped how many AI runs the site may make this calendar
month, and the cap is reached.

An admin can raise or clear it under *Monthly limit*, and the reading is
shown by **Check usage this month**. Otherwise the allowance resets on the
1st, UTC.

The cap applies to administrators too, so seeing this as an admin is normal —
raise the number and carry on.

## "This action requires Jira administrator permissions"

Settings are admin-only, including reading them. Anyone who can see an issue
can run assessments; only an admin configures the site.

## Assessments are generic and could apply to any project

Working as designed, and fixable. In order of payoff:

1. **Point it at your architecture page** — one Confluence link. Mitigations
   start naming your actual components instead of giving textbook advice
2. **[Import your incidents](incidents.md)** — risks start citing what
   happened to you, by issue key
3. **Turn on comments** under *Context depth* — decisions and caveats
   frequently exist nowhere else. It costs tokens and it is usually worth it
4. **Write a standing instruction** for what is permanently true about your
   context

An assessment on a one-line issue description will be thin no matter what.
The model cannot know what was never written down.

## Nothing appears in the Code risks tab

Work through it in this order:

| Check | |
|---|---|
| Does the branch, PR title or PR description contain the issue key? | No key, no assessment. `BT-12-fix-webhook` or `[BT-12] ...` |
| Did the Action run at all? | Look at the workflow run in GitHub |
| Did it log a 401? | `PREMORTEM_CI_SECRET` is wrong or missing |
| Did it say "no CI signing secret"? | The site never fetched the webhook URL. ⚙ → Apps → Premortem settings → CI integration |
| Did it succeed and still nothing? | Correct. CI only stores the diff — open the issue and click **Assess code risks** |

## The panel shows nothing on an issue nobody has assessed

That is the resting state. Before the first assessment there is nothing to
show, so the panel offers **Assess risks** and stays quiet — an empty list of
risks would read as "assessed, and clean".

## The pipeline broke right after rotating a secret

Expected. Rotating the CI secret or regenerating the webhook URL invalidates
the old one immediately. Update the GitHub secret — one organization secret
update, not one edit per repository.

## Imported incidents are being ignored

The settings page shows a count of incidents embedded with a **different**
provider than the one now configured. Those are skipped, because vectors from
different models cannot be meaningfully compared.

Re-import the project.

## The register has rows for issues that no longer exist

⚙ → Apps → Premortem settings → **Register maintenance**. It also runs on a
schedule; the button is for when you want it correct now.

## Writing the same issue twice duplicated the risks

It should not — rows and the description section are replaced, not appended.
If you are seeing genuine duplicates, that is a bug worth reporting with the
issue key and the register page.

## Two Premortem panels on the issue

One is a development build. Only one should be installed on a production
site; if you see a panel badged **DEVELOPMENT**, an admin installed a dev
version of the app on this site.
