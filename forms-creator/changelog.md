# Forms Creator — Changelog

Permanent record of user-facing changes. Never prune this file.

## [2026-05-11] Language modes, AI translation, modal editors

- Added per-form language mode: English-only, Spanish-only, or Bilingual
- Added AI-powered translation via Google Gemini (Generate Spanish, Generate English, Translate All banner)
- Translate All now works in both directions (EN→ES and ES→EN)
- Smart prompt when switching language modes warns before clearing translations
- Label editing now uses a polished modal dialog (replaced inline popover)
- "Regenerate" button in label modal re-runs AI translation for individual fields
- Added field modal for radio/dropdown options (add, remove, reorder EN/ES pairs)
- Add Field flow replaced with a modal dialog and visual field-type picker
- Section headings are now editable (click to open label editor)
- Required toggle added per field directly on the edit page
- Dashboard now shows templates as cards with descriptions; internal dev templates hidden
- Published Bilingual forms include EN/ES language toggle; single-language forms do not
- Prerequisites updated: GOOGLE_API_KEY in .env required for AI features

## [2026-05-08] Initial release (Phase 1 MVP)

- Forms Creator web app launched
- Clone-from-template workflow on dashboard
- Direct-edit UI for form title, description, field labels (EN/ES)
- Add / remove / reorder fields with drag-and-drop
- Outputs config panel (publish path, Google Sheet ID)
- One-click Publish generating HTML web form + Apps Script
- Preview mode with language toggle (bilingual forms)
- Initial user guide created
