# Changelog

## [2026-06-25]
- Add `salary_schedule_tagfix.py` -- new standalone tool for in-place ADA remediation of Excel-export salary schedule PDFs. Excel exports arrive tagged (Document/Workbook/Worksheet/Table) but ADA-broken: no TH header cells, title band hidden as an /Artifact invisible to screen readers, footnotes/headings trapped in single-column tables. Fixes all defects in place; render stays byte-identical to source (verified on every run via md5 pixmap comparison). Fixes applied: data table header row -> TH Scope=Column; first data cell per row -> TH Scope=Row; label rows ("Group AA / Range 01") -> Caption; 1-col single-row tables -> H2; 1-col multi-row tables -> L/LI/LBody list; title band un-artifacted -> H1+P; /Lang, /Title, /DisplayDocTitle, /Tabs /S per page set. Validated against hand-remediated GRP 21 reference (byte-identical).
- Extend `salary_schedule_tagfix.py` to handle native Excel Save As PDF format in addition to Acrobat PDFMaker. PDFMaker exports: cells are TD>MCID, title band in /Artifact BDC with one block per column (Creator: "Acrobat PDFMaker for Excel"). Native Save As exports: cells are TD>P>MCID, title band in /Artifact BMC with one block per text line (Creator: "Microsoft Excel"). Three-part fix for native flavor: (1) detect BMC artifact, pick first text-bearing one, cluster blocks into columns by x-gap (leftmost -> H1, rest -> P); (2) move cell's child-content (P nodes) rather than extracting bare MCIDs when building Caption/H2/L elements; (3) inherit /Pg down the tree during ParentTree rebuild (P elements in cells lack /Pg). GRP 26 and GRP 27 salary schedules verified: 30 scoped TH, 1 caption, H1+P+P title band, 2 footnote lists, render byte-identical.

## [2026-06-10]
- Extend underscore fill-in line artifacting to Google Docs PDF exports. Google Docs uses 2-byte Type0/Identity-H fonts where the underscore glyph is encoded as a font-specific code (e.g. 0x0042) rather than ASCII 0x5F. New CID pass in `underscore_artifact.py` reads each font's `/ToUnicode` CMap, finds the underscore glyph code, and artifacts matching BT...ET groups the same way the existing byte-level pass does for Word exports.
- Fix empty `<P>` tag removal for Google Docs exports. The CID space glyph (e.g. `<0003>`) contains two non-zero bytes that survived the old `bytes.strip()` check, leaving whitespace-only elements in the tag tree. `clean_empty_tags` now decodes through each font's ToUnicode map and treats draw ops (`Do`, path operators) as content, so glyph-only whitespace elements are correctly removed and their marked-content blocks are converted to `/Artifact`.

## [2026-06-03]
- Add `autotag.py` — new standalone tool for untagged PDFs that have a real text layer (graphically-designed flyers from Canva, InDesign, etc. that Acrobat Autotag handles poorly). Uses Claude vision per page to read text off the rendered image (corrects garbled source text layers), derive reading order, and assign heading/paragraph roles. Builds the tag tree via the invisible-overlay technique: artifacts the original content, adds an invisible tagged text layer with `/ActualText` (full Unicode), sets `/MarkInfo`, `/Lang`, `/Title`, `/Tabs /S`. Interactive confirm step lets you review and adjust the detected structure before writing. Flags: `--lang` (default `en-US`), `--title` (override), `--yes` (skip confirm). Batch mode via folder path.
- Add `--images classify|alt|decorative` flag to `remediate.py` — replaces the old always-alt-text figure behavior. `classify` (default): Gemini AI classifies each `<Figure>` as DECORATIVE or CONTENT, prints a table, and at a TTY lets you flip verdicts before applying. Decorative images are artifacted (struct element removed, MCID retagged to `/Artifact`, ParentTree slot nulled). Content images get `/Alt`. `decorative`: artifact all figures, no AI. `alt`: legacy always-alt-text.
- Add tab order step (Step 2b) to `remediate.py` — sets `/Tabs /S` on every page unconditionally. Word exports don't always set it; Acrobat's "Tab order" accessibility check fails without it. Both `remediate.py` and `autotag.py` now guarantee this is present.
- Fix `autotag.py` heading normalization: headings before the first H1 (flyer kickers) demoted to P; level skips clamped; first heading promoted to H1 if none exists — passes Acrobat "Appropriate nesting."
- Fix `autotag.py` character encoding: invisible-layer Helvetica now carries a generated WinAnsi→Unicode `/ToUnicode` CMap; text passed to the glyph layer has newlines/tabs collapsed to spaces (byte 0x0A was unmapped and failed Acrobat "character encoding").
- New module `figure_classify.py` — decorative vs. content classification logic extracted from the figure step; reusable and tested independently.

## [2026-05-08]
### Added — 2026-05-08 (later)
- `--review-tables` flag: detects matrix-style tables Word emitted with empty
  TH corner cells, asks Claude to propose a Table→List restructure, queues
  each proposal to `output/<basename>_remediated.review.json` for review
  (paired with rendered page images).
- New `review <queue.json>` subcommand: walks the queue interactively
  (accept / skip / note-and-skip / quit per entry, persists progress after
  every decision so Ctrl-C is safe), then applies accepted restructures
  to the remediated PDF by mutating the tag tree (Table → L/LI with
  /ActualText on data sub-LIs to inject column-header context).
- Restructure entries are appended to the existing
  `output/<basename>_remediated.fixes.json` audit log alongside any
  `--vision-fix` entries.

- Add `--vision-fix` CLI flag to `remediate.py` — opt-in pass that uses Claude vision (Anthropic API) to fix two ADA issues the rule-based steps can't reach: Tables > Regularity (cells with mismatched ColSpan/RowSpan that fail the row-width audit) and Appropriate Nesting (e.g. an `<H3>` Word stamped onto a closing courtesy phrase that shouldn't be a heading at all). Runs as Step 6 (table regularity) and Step 7 (heading hierarchy) after the existing 5 steps; each step is wrapped so a failure in one doesn't block the save. Requires `ANTHROPIC_API_KEY` in `.env` and internet access; without the flag the tool's behavior is byte-for-byte unchanged
- Sidecar audit log written to `output/<basename>_remediated.fixes.json` whenever `--vision-fix` runs — records every applied/skipped change with Claude's reasoning, so reviewers can spot-check decisions without re-running the model
- Add checkbox + radio AcroForm widgets to the rebuild pipeline — `rebuild/form_fields.py` gains `add_checkbox` and `add_radio_group` helpers; `rebuild/add_form_widgets.py` learns to inject `/Btn` widgets anchored on the right side of invisible `[[F:Name]]` (checkbox) and `[[F:Group=Value]]` (radio) markers
- Visible glyphs are Unicode `☐` (U+2610) and `○` (U+25CB) in the docx body font — the static-print case (skip `add_form_widgets.py`) still produces a clean printable
- Appearance streams live in the new `rebuild/_appearance_streams.py`. The off-state Form-XObject is intentionally empty so the docx glyph beneath supplies the visible outline; the on-state paints only the indicator (checkmark or filled dot) over the glyph. This avoids the doubled-outline rendering that came from drawing the widget border alongside the typeset glyph
- `find_markers` now uses PyMuPDF `rawdict` per-character bboxes so the widget rect overlays the actual ☐/○ glyph ink rather than the line-height bbox of the whole word

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
