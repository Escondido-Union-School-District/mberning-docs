# Changelog

All changes to this project, logged automatically by the Daily User Guide Check.

## [2026-04-27]
- Add Step 5 to remediate.py — artifact-wraps runs of 3+ underscore characters (Word tab-leader signature lines) so screen readers skip them while the visual line remains
- Add underscore_artifact.py module (port of pdf-form-prep/prep_form.py logic)
- Add Quantum Glow Dance permission slip rebuild script (rebuild/quantum_glow_dance_permission_slip.py)

## 2026-04-22
- Update user guide — prune stale 2026-03-19 changelog entries, add rebuild/ folder to Folder Structure section

## 2026-04-14
- Add rebuild/ workflow for PDFs too broken to patch with remediate.py — produces a clean Word document that exports to a properly tagged PDF

## 2026-04-08
- Fix changelog table cell background color
- Automated guide update

## 2026-04-07
- Remove print link
- Update guide styling — title, label, and changelog formatting
- Backfill changelog — last 30 days of commits
- Replace print button with simple link
- Remove 'Last project activity' line — replaced by automated changelog
- Update user guide — add changelog section and permanent changelog for batch mode and Windows file lock fixes


## 2026-03-27
- fix: use Path.replace() for cross-platform processed file move; add user guide and CLAUDE.md

## 2026-03-23
- feat: move processed input files to processed/ folder

## 2026-03-19
- chore: add input/ and review/ to gitignore
- refactor: simplify table headers to CLI flag (--headers row/column/both/none)
- feat: interactive table header fix â€” saves screenshots and prompts user for R/C/B/S
- feat: Gemini-driven table header detection (row, column, or both) with manual review fallback
- feat: fix table headers by changing first row TD cells to TH with Column scope
- fix: update Gemini model to gemini-2.5-flash (2.0-flash no longer available)
- fix: extract full multi-line H1 title by matching consecutive lines with same font
- fix: prefer PyMuPDF for H1 title extraction to avoid garbled content stream text
- feat: set document title from first H1 tag with DisplayDocTitle
- feat: set document language to en-US
- feat: Gemini vision integration with new google-genai SDK
- feat: fallback image extraction via PyMuPDF page screenshot
- feat: primary image extraction via content stream MCID parsing
- feat: tag tree walking to find Figure and other structure elements
- feat: main entry point with input validation, PDF open, and summary output
- chore: project scaffolding with dependencies and script skeleton
- docs: design spec and implementation plan for PDF accessibility remediation

## 2026-04-06
- Added batch mode (pass folder path to process all PDFs at once), fixed Windows stdout buffering and file lock handling with retry logic
- feat: add batch mode and fix Windows file locking issues
