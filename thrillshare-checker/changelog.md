# Changelog

All changes to this project, logged automatically by the Daily User Guide Check.

## 2026-05-01
- Smore scanner rewritten to cover every school the account can see (was: hardcoded 3-school principal allowlist)
- No sender filter — picks up principals, office managers, and district staff alike
- Lookback window shortened from 36h to 26h (daily schedule + small overlap for delayed starts)
- Sender column auto-discovered per school from "Published By" table header (resilient to layout changes)
- Fixed "Applying filters" overlay being misread as a message row date
- Fixed UTF-8 stdout crash on Windows cp1252 console (was silently killing Miller Elementary's run on non-breaking hyphens/smart quotes)

## 2026-04-16
- Both checks now always send email after every run; subject line reflects result ("Posts Pending Approval" vs "Check Complete — No Pending Approvals")
- Replaced VBS wrappers (`run_hidden.vbs`, `run_smore_scan.vbs`) with BAT files (`run_hidden.bat`, `run_smore_scan.bat`) — enables S4U logon so tasks run without an active user session
- Updated user guide: scheduling table, email behavior description, folder structure, troubleshooting

## 2026-04-15
- Extended hung-run protection to smore scan path (`run_smore_scan.vbs` now runs `cleanup_stale.ps1` first)
- `cleanup_stale.ps1` now also kills stale `wscript.exe` processes (previously only python + chrome-headless-shell)
- Rescheduled: smore scan moved to 3:00 AM + 6:50 AM; ThrillshareChecker 7:00 AM trigger replaced with 4:00 AM + 7:20 AM (hourly 6–4 unchanged)
- Both tasks set to `StartWhenAvailable=True` and `LogonType=Interactive`

## 2026-04-14
- Added 15-min watchdog timer to `checker.py` that force-kills the process tree if Playwright hangs
- Added `cleanup_stale.ps1` to kill stale python/chrome-headless-shell processes (>30 min) before each scheduled run
- `run_hidden.vbs` updated to run cleanup before the checker

## 2026-04-13
- Smore scan now sends an error email on login failure or unhandled exception (previously failed silently)
- Added per-school try/except in smore scan with page crash recovery — continues to next school on Chromium crash

## 2026-04-08
- Fix changelog table cell background color
- Automated guide update

## 2026-04-07
- Remove print link
- Update guide styling — title, label, and changelog formatting
- Backfill changelog — last 30 days of commits
- Replace print button with simple link
- Remove 'Last project activity' line — replaced by automated changelog
- Update user guide — add changelog section and permanent changelog for Smore Link Scanner feature


## 2026-03-19
- Add CMS moderation checks, SMTP relay, date-organized reports
- Fix run_hidden.vbs paths after project move to eusd subdirectory

## 2026-03-11
- Add report file output, longer school switch wait, scheduled task
- Working Thrillshare approval checker with cookie-based sessions
- Initial implementation of Thrillshare approval checker

## 2026-04-06
- Added --smore-scan flag to find Smore links in principal emails
- Added daily scheduled Smore scan with 36-hour lookback, scheduled at 7:00 AM
- Updated user guide with Smore Link Scanner section
- chore: change smore scan lookback to 36 hours, schedule at 7 AM
- feat: daily scheduled Smore scan with 24-hour lookback
- feat: add --smore-scan flag to find Smore links in principal emails
