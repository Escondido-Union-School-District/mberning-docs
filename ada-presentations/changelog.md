# ADA Presentations — Changelog

## [2026-04-28] Fix bullet list detection in text boxes
- Bullet lists inside free-floating text boxes (common in Google Slides exports) now correctly produce `list` blocks instead of individual `paragraph` blocks
- Detection reads XML bullet markers per paragraph: `buChar` → unordered list, `buAutoNum` → ordered list, `buNone` → explicit no-bullet
- Body placeholder paragraphs without an explicit marker still default to list items (inheriting from slide layout, as before)
- Consecutive bulleted paragraphs are grouped into a single list block; a non-bulleted paragraph between two bulleted groups correctly splits them into two separate list blocks
- Nested indent levels (e.g., sub-bullets) are preserved in the output HTML

## [2026-04-22] Fix H1 ordering and image overflow in preview deck
- H1 heading now always appears first in the block list so decorative images placed above the title in the PPTX don't precede it for screen readers or visually
- `figure` max-width capped at 30% and `img` max-height at 25vh so multi-image slides no longer overflow the viewport

## [2026-04-22] Fix blank preview deck
- Removed `<main>` wrapper around `.reveal`; reveal.js requires a viewport-sized parent, so `role="main"` is now set directly on the `.reveal` element
- Preview page now loads and navigates correctly

## [2026-04-22] PPTX-native pipeline rewrite
- Replaced PDF screenshot + AI reconstruction pipeline with python-pptx structural extraction
- Real headings, lists, tables, speaker notes, and author alt text read directly from the .pptx file
- AI Vision (Claude/Gemini) now used only for images without author alt text, chart summaries, and QR fallback
- LibreOffice + PyMuPDF renders slide preview PNGs for the editor
- zxing-cpp decodes QR codes embedded in slides; clickable links emitted in output HTML
- Per-project theme mode: EUSD branding (royal blue/gold/Arial) or preserve-original with WCAG contrast enforcement
- Archived PDF pipeline under `app/services/legacy/` for reference
