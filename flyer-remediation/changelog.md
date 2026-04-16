# Flyer Remediation — Changelog

## [2026-04-16] v2 text-first pipeline and accessibility improvements

- **v2 text-first pipeline (default):** Replaced AI region-detection approach with fitz text
  extraction + AI semantic classification. Produces correct tags on dense Canva flyers.
  Auto-detects document language; Spanish flyers get Spanish alt text.
- **Heading hierarchy enforcement:** AI prompt strictly prohibits skipping heading levels
  (e.g. H1→H3 without H2), preventing ADA violations.
- **Text quality checks:** Automated word-boundary/encoding check + Vision cross-check
  against page image, run on every element before saving.
- **Link role and QR code support:** Text containing URLs tagged as Link. QR code images
  decoded by AI with target URL written into alt text.
- **PDF bookmarks:** H1–H4 headings automatically added as PDF bookmarks (fixes PAC 2024
  quality check failure).
- **Clickable link annotations:** Link elements also create a clickable URI annotation so
  users can navigate to URLs from any PDF viewer.
- **H1 always first:** H1 moved to reading-order position 0 regardless of visual position.
- **Font size capping improved:** Per-character width estimate widened to 0.65 to prevent
  off-page truncation on all-caps Spanish text.
- **Image fallback for Form XObjects:** Photos inside Canva Form XObjects (no direct Do op)
  now get an invisible marker at the image bbox so they appear in the tag tree.

## [2026-04-16] Initial user guide created

- Created `docs/user-guide.html` with full CLI reference, review workflow, and troubleshooting.
