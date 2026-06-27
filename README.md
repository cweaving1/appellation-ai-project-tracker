# Appellation AI Project Tracker

Live, multi-user project tracker for Claude, Cowork, and AI initiatives across the Appellation Hotels portfolio. Built as a single static HTML page that talks directly to a Supabase Postgres database — no server, no logins.

**Live dashboard:** https://<username>.github.io/appellation-ai-project-tracker/

Replace `<username>` with the GitHub account (or org) this repo lives under once it's deployed.

## Architecture

- **Frontend:** `index.html` — single self-contained file (HTML + inline CSS + inline JS). No build step. Deployed via GitHub Pages.
- **Backend:** Supabase project `afkjchshoktlmqrpopyk`
  - Table `wishlist_items` — one row per idea / project
  - Table `wishlist_comments` — running thread of comments per item
  - Row-Level Security: public read + insert + update for `anon` role; no public delete
  - Publishable / anon key is embedded in `index.html` — safe by design (publishable keys are meant for client code)

## Editing

- **Content tweaks** (titles, layout, copy): edit `index.html`, commit, push. Live in ~30 seconds.
- **Schema changes** (add a column, change a dropdown option): apply via Supabase SQL Editor, then update the dropdown constants near the top of `index.html`.
- **Data cleanup** (delete junk submissions): use Supabase Table Editor. The anon role can't delete via the dashboard, so cleanup happens admin-side.

## Dropdowns

Defined in `index.html` near the top of the `<script>` block. Edit the JS arrays to change the lists:

- `SUBMITTERS` — who can submit
- `DISCIPLINES` — Marketing, Web, Email, Paid Media / Ads, Operations, F&B, Spa, Reservations, Sales, Finance, IT, HR, Brand, Other
- `PROPERTIES` — Appellation Healdsburg, The MOHI, Lodi / Wine & Roses, AMEYALLI Park City, Petaluma, Pacific Grove, Brand-wide, Multi-property
- `PRIORITIES` — High, Medium, Low
- `STATUSES` — Submitted, Reviewing, Approved, In Progress, Blocked, Done, Declined
- `EFFORTS` — Small, Medium, Large
- `OWNERS` — Unassigned, Christa, Courtland, Eric

## Security model

The dashboard is "URL = access" — anyone with the link can view, submit, and edit. Share the URL only via internal channels. The Supabase publishable key is fine to expose; it only grants what RLS allows (read, insert, update — no delete, no schema changes, no access to other tables).

If the URL ever leaks externally and we start seeing junk submissions, the recovery path is to tighten RLS in Supabase (require Supabase magic-link auth) and update `index.html` to add a sign-in step.

## Operations

- **Weekly digest** (planned for v1.5): Cowork scheduled task that runs every Monday morning, queries new + changed items in the last 7 days, and emails Christa, Courtland, and Eric.

## Repo history

Built 2026-06-27 by Christa Weaving with Claude (Cowork mode). See `CLAUDE.md` or session transcript for the build sequence.
