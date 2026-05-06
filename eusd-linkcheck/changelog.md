# Changelog

All changes to this project, logged automatically by the Daily User Guide Check.

## 2026-05-06
- Mirror latest full-crawl reports to `docs/reports/` with stable filenames (no timestamp); `docs/reports/index.html` is always the latest landing page
- Combined landing page: `docs/reports/index.html` is now the searchable Pages Index (search, site filter, reverse-link lookup, broken-link drill-down) with a nav strip on top
- Nav strip made explicit: full report names, "You are viewing" current-view label, "Switch to →" prefix, hover tooltips, red/amber pills when counts are nonzero
- Added monthly automated crawl via Windows Task Scheduler ("EUSD Link Checker Monthly") — runs `run-monthly-crawl.cmd` on the 1st of each month at 06:00 PT with StartWhenAvailable enabled; logs to `output/monthly-crawl-YYYY-MM-DD.log`
- Landing page nav now shows both last-crawled time and next-scheduled-crawl date

## 2026-04-28
- Pages-index: add broken-link drill-down (click badge to expand inline list) and "Only with broken links" filter checkbox
- Add pages-index report with one row per crawled page, reverse-link lookup, site filter, and `npm run regen:pages` offline rebuild
- Capture external links in link-inventory (previously internal-only); wait for JS hydration on Next.js pages before extracting anchors

## 2026-04-08
- Fix changelog table cell background color
- Automated guide update

## 2026-04-07
- Remove print link
- Update guide styling — title, label, and changelog formatting
- Backfill changelog — last 30 days of commits
- Replace print button with simple link
- Remove 'Last project activity' line — replaced by automated changelog


## 2026-03-27
- Add persistent disk cache and CLAUDE.md; tune concurrency defaults
