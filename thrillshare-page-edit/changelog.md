# Thrillshare Page Edit — Changelog

## [2026-05-06] Cross-school link inference and discovery improvements
- Added cross-school link inference in `--apply` mode: image buttons with no link URL are matched against captioned buttons on other schools' pages to infer the correct destination URL
- Discovery now handles "Parents and Families Links" as a valid Resources page title variant
- CMS page list endpoint now follows pagination (previously only fetched first page of results)
- Added support for nested layout content shapes; broadened Resources-page regex to catch more naming variants
- Glen View Elementary's content structure and "Information for Families" page title now supported
- Unresolved unlinked buttons (where inference finds no match) are surfaced in the apply report for manual follow-up

## [2026-04-30] Full CLI and apply/report pipeline
- Added `--apply` mode: two-pass bulk apply across all schools with report saved to `reports/`
- Added `--dry-run` mode (default): apply to one chosen school/page and open in browser for visual verification
- Added `--discover-only` mode: list all discovered Resources pages without making edits
- Added `--school` and `--page` flags to target specific pages in dry-run
- Added `--no-browser-verify` flag to skip browser preview in dry-run
- Caption text generation: Gemini Flash OCR (primary) with URL-slug fallback; report marks URL-slug captions with `*` for manual review
- Report format includes EDITED / SKIPPED / ERRORS / UNRESOLVED sections

## [2026-04-29] Initial tool
- Project scaffolded with auth (Playwright Google SSO + cookie session), API client, page tree normalizer, plan/idempotency logic, discovery (fuzzy page title matching), and CLI entry point
- Cookies saved to `session/cookies.json` and reused on subsequent runs
- Environment-backed config via `.env` (GEMINI_API_KEY required)
