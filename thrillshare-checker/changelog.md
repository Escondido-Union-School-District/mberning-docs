# Changelog

All changes to this project, logged automatically by the Daily User Guide Check.

## 2026-06-29
- Scheduled tasks corrected to weekdays only (Mon-Fri); previous docs said "daily" but triggers are MSFT_TaskWeeklyTrigger with DaysOfWeek=62 (Mon-Fri, no weekends)
- WakeToRun enabled on both ThrillshareChecker and Thrillshare Smore Scan tasks (was False); the PC sleeps after 2h idle on AC power and early-morning triggers were silently skipped without this; wake timers were already allowed on the power scheme
- cleanup_stale.ps1: reap orphaned chrome-headless-shell by name + age instead of CommandLine match; processes spawned by a prior S4U-scheduled run live in a different logon session where CommandLine reads as null, so the old filter matched nothing and orphans accumulated; added taskkill fallback for Stop-Process cross-session denials
- run_hidden.bat, run_smore_scan.bat: added Python -u flag so log.txt / smore_scan_log.txt capture live per-school output during scheduled runs (without -u, stdout is block-buffered and the log file stays empty until process exit)

## 2026-05-28
- Approval checker now tolerates the Thrillshare media SPA's OAuth handshake delay on load (15-20s observed)
- Raised the three `.dropdown-trigger` waits to a shared 30s `DROPDOWN_TIMEOUT` (was a few seconds each); happy path is unaffected since wait returns as soon as the element appears
- Added one retry for the initial CMS pages `goto` on timeout; the OAuth handshake on first navigation can exceed the nav timeout, after which the session is warm
- Fixes intermittent "No schools discovered" false negatives that were appearing in hourly scheduled runs

## 2026-05-26
- When `--smore-scan` discovers a new Smore URL, it now automatically queues a background ADA accessibility scan via the local smore-ada-checker project (`check.py --scan-only <url>`)
- Scan results appear on the smore-ada-checker dashboard at `http://localhost:5051/dashboard` for review — no additional email is sent
- New module: `smore_checker_handoff.py` handles subprocess spawning; uses smore-ada-checker's own venv Python
- New config: `SMORE_CHECKER_DIR` in `.env` points at the smore-ada-checker installation (default: `C:\Users\mberning\projects\eusd\smore-ada-checker`); set to empty string to disable handoff

## 2026-05-15
- Smore scanner now runs a 24h/48h follow-up pass: re-fetches every discovered Smore URL whose initial snapshot is ~24h or ~48h old, emails a unified diff if content changed, or an "unreachable" notice if the URL is down
- Added `--no-followup` CLI flag to skip the follow-up pass and run only the initial discovery scan
- Lookback window extended from 26h to 168h (7 days) for row-text extraction; popup expansion remains 48h (`POPUP_LOOKBACK_HOURS`)
- Added `smore_snapshot.py` module for Playwright-based page capture and text normalization (view counters stripped to reduce false-positive diffs)
- Added `session/smore_snapshots/` directory: plain-text page snapshots keyed by sha1(url), pruned when entries expire from `seen_smore.json`
- `seen_smore.json` now tracks `snapshot_sha`, `checks_done`, and `follow_up_status` per URL
- Orphan snapshot files swept each run to keep the snapshot directory in sync with seen_smore.json
- Fixed smore scanner missing alerts where popup state leaked between schools

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
