# Moving from version 3.x to 4.0.0

This page is for sites that installed Premortem before 29 October 2026, while
it was an unlicensed app. If you installed from the Marketplace on or after
that date, it does not apply to you.

## What you will see

Your Jira admin's **Connected apps** page shows Premortem as **Outdated
3.10.0** with a **Paid update** badge, and the only button it offers is
**Uninstall**. There is no Update and no Approve.

That is not a bug in your site and not something we can fix from our side. An
unlicensed installation of a Forge app cannot become a licensed one: Atlassian
refuses the upgrade outright. Version 4.0.0 is licensed through the
Marketplace, so the only route to it is to uninstall and install again from
the listing.

## What uninstalling costs you

Read this part before you do anything. Uninstalling clears the app's storage
on your site, and reinstalling starts it empty.

**Lost, and not recoverable by us or by you:**

- the provider choice and the API key, which you will paste in again
- the monthly limit and every other setting
- **every imported incident and postmortem, with the vectors built from
  them** — this is the expensive one. Re-importing costs the embedding calls
  again, on your key
- the CI signing secret, so a connected pipeline must be given the new one
- accept, dismiss and *This happened* history, and any risk you added with
  *It missed one*

**Kept, because it never lived in the app:**

- every risk you wrote into an issue description with **Write accepted** — it
  is ordinary issue text
- your Confluence risk register, which is ordinary Confluence pages

So the risks you decided to keep survive. The app's memory of how it got
there does not.

## If you decide to move

1. Note your settings first: provider, model if you set one, architecture
   page link, monthly limit, register space. One screenshot of the settings
   page is enough.
2. Note which projects you imported incidents from, and which Confluence
   pages you imported postmortems from.
3. Uninstall Premortem from **Connected apps**.
4. Install it again from
   [the Marketplace listing](https://marketplace.atlassian.com/apps/3455340266/premortem-risk-assessment-for-jira).
   Free for teams of ten or fewer; $3 per user per month above ten.
5. Paste the key back, press **Test connection**, and re-import the incidents
   and postmortems from step 2.
6. If you use the GitHub Action, copy the new CI secret into your repository
   secrets — the old one is gone.

## If you decide not to move

3.10.0 keeps working on your site. It will not receive fixes or new features,
and we cannot reach it to tell you anything, which is the reason this page
exists on the public site rather than in a release note.

## What it is reasonable to be annoyed about

This is a one-time cost created by us turning licensing on, and it falls on
the people who installed earliest. If the re-import is the part that stops
you, write to support@usepremortem.com and say which projects and pages you
had imported — we cannot restore them, but we can tell you what the re-import
will cost in provider calls before you start it.
