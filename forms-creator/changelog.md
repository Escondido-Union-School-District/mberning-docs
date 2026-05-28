# Forms Creator — Changelog

Permanent record of user-facing changes. Never prune this file.

## [2026-05-27] FERPA/NDPA-equivalence controls (Phase 1 + Phase 2)

- Annual confidentiality acknowledgement: staff see a one-page notice once per year before accessing the app; must click "I Acknowledge" to continue
- Per-field PII Role tag in the Add Field / Edit Field dialog (Student Name, Student ID, Parent Name, Parent Contact, Demographic, Medical)
- Email notifications now automatically redact values in PII-tagged fields from the email body
- Per-submission delete: form owners and admins can permanently delete individual submissions from the submission detail page
- Per-form delete: form owners and admins can permanently delete a form and all its submissions from the edit page; cascades to PDFs and published bundle files
- Admin: student-records page at /admin/student-records — search submissions by student name, export parent-inspection bundle (ZIP), or delete all submissions for a named student
- Admin: confidentiality acknowledgements viewer at /admin/confidentiality — see which staff have completed the annual acknowledgement
- Public /privacy page available at /privacy without signing in — plain-language summary of data collection, protection, and parent rights
- Security headers added: HTTPS enforcement, HSTS, secure/HttpOnly/SameSite cookies
- Footer links added on all pages: /privacy (always), /admin/audit (admin-only)
- Audit log now records form ACL denials (auth.denied) in addition to submission and auth events

## [2026-05-20] Google sign-in, form sharing with role groups

- Google OAuth sign-in required; only @eusd.org accounts can sign in
- Branded sign-in page and signed-out confirmation page
- Sign-out chip (email + Sign out link) added to top-right of all pages
- Form sharing panel on edit page: share by individual @eusd.org email or by staff role checklist (Admin Staff, Principal, Office Manager, etc.)
- Role checklist auto-detects your school from your Google group memberships
- Shared users can view and edit the form; only the owner can manage sharing
- Admin: unassigned-forms section on dashboard for legacy forms with no owner; admins can assign ownership by email
- Dashboard form cards now show an owner chip

## [2026-05-15] Per-form branding, 8 web layouts, dashboard redesign, submission improvements

- Per-form Branding panel: school name, logo upload (up to 200 KB), primary color picker, accent color picker
- 8 WCAG-compliant web form visual styles: Classic, Cards, Minimal, Paper, Friendly, Varsity, Compact, Editorial
- All styles apply school brand colors automatically
- Mobile/Desktop toggle in the live preview pane
- Dashboard My Forms redesigned: card per form with submission count and amber "N new" pill; stacked Edit/Submissions/Archive buttons
- Submissions inbox: student name and grade columns (when fields are tagged appropriately); Response totals block for checkbox/radio fields
- Cross-form master submissions view at /submissions (linked from dashboard)
- Submissions inbox polish: human-friendly Pacific Time timestamps throughout

## [2026-05-14] Server-based submissions (Apps Script removed)

- Parent submissions now POST to and are stored on the forms-creator server (SQLite)
- Per-form submissions inbox: list, search, filter, mark reviewed, add notes, download PDF, export CSV
- Archival PDF generated for each submission (PDF/UA-tagged)
- Apps Script / Google Sheets output removed entirely
- Email notification on each new submission (configurable per form; requires SMTP server config)
- Published bundle now bakes the submission URL from FORMS_CREATOR_PUBLIC_URL

## [2026-05-12] Live preview, new field types, dashboard templates, section management

- Live inline side-by-side preview on edit page; refreshes on every save
- Mobile/Desktop viewport toggle in preview pane
- Subheading field type: bold sub-title within a section (renders in web form and PDFs)
- Static text / Paragraph field type: instructional block not a fillable field
- + Add Section button with naming dialog
- + Add Paragraph button for heading-less instructional blocks
- Archive / Restore per form (collapsible "Archived" section on dashboard)
- Dashboard: 6 new K-8 office templates (emergency contact, transportation change, field trip chaperone, lost and found, permission slip, allergy waiver)
- Blank form template added as prominent "+ Blank form" card
- Starter template (example of every field type) added
- Translate count semantics fix: empty fields no longer counted as missing translations
- Confirm dialog for destructive actions (delete field) now uses styled modal instead of native browser prompt

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
