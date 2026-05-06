# Changelog

## [2026-05-06]
- Add fillable-form pass — `rebuild/add_form_widgets.py` injects AcroForm text widgets (single-line + multi-line) into a tagged PDF, anchored by invisible `[[F:Name]]` markers placed in the .docx by the rebuild script. Widgets are tagged in the structure tree as `/Form` elements with `/OBJR` children (PDF/UA Matterhorn 28-010); pages with widgets get `/Tab /S` for structure-order tabbing (Matterhorn 28-002); `/AcroForm/DR/Font/Helv` is populated so fields render on first focus
- Add `rebuild/form_fields.py` — helpers (`add_invisible_marker`, `add_field_fill_line`, `write_fields_json`) used by rebuild scripts to declare fillable fields and emit a sidecar `.fields.json`
- Wire `rebuild/waiver_form.py` to emit fields — same .docx now drives both static-print and fillable-PDF pipelines (markers are 1pt white-on-white, so static print output is unchanged)
- Lift shared struct-tree helpers (`get_kids`, `find_struct_elements`, `get_mcids`, `page_index`) into `rebuild/_pdf_struct.py` — `remediate.py` and `underscore_artifact.py` now import from it
- Fix pre-existing crash in `underscore_artifact.clean_empty_tags` when `StructTreeRoot/K` is an Array (Word PDFs vary; was masked until widget-injection touched the tree)
- New pipeline order: `waiver_form.py` → Word Save As PDF → `add_form_widgets.py` → `remediate.py`. Skip `add_form_widgets.py` to get the static printable PDF instead

## [2026-05-05]
- Add Allergy Waiver bilingual rebuild — `rebuild/waiver_form.py` generates `Allergy_Waiver_English.docx` (en-US) and `Allergy_Waiver_Spanish.docx` (es-ES) from original single bilingual PDF; logos extracted from source PDF via PyMuPDF rasterization to avoid color-profile artifacts
- Add `rebuild/verify_page_count.py` — drives Word via COM to confirm each rebuilt .docx is exactly 1 page before PDF export; exits non-zero if any file exceeds the limit
- Add page-count rule and per-step tightening ladder to `waiver_form.py` docstring; add page-fit constants (BODY_PT, H1_PT, PARA_AFTER_PT, WRITE_AFTER_PT, PAGE_MARGIN_IN) so future editors adjust one place
- Add `python-docx` to `requirements.txt` (was already needed by all rebuild scripts but missing from the file)

## [2026-04-28]
- Add FER guidelines bilingual rebuild — `rebuild/fer_guidelines.py` produces `FER_Guidelines_English.docx` and `FER_Guidelines_Spanish.docx`; source PDF had StructTreeRoot but no MarkInfo and no heading tags so remediate.py could not process it
- Document that Word's built-in Save As PDF must be used for alt text to carry through to the PDF tag tree — Adobe PDFMaker does not reliably transfer `wp:docPr/@descr` alt text to the `<Figure>` /Alt entry
- Add troubleshooting note: "PDF is not tagged" error can occur on PDFs with a structure tree but missing MarkInfo flag (older Acrobat PDFMaker exports); rebuild workflow is the correct path for these

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
