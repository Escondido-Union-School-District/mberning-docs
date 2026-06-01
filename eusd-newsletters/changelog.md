# EUSD Newsletters — Changelog

Permanent log of user-facing changes. Never pruned.

---

## [2026-06-01] Phase 3b complete — Content Accessibility Engine (core)

- Content Accessibility Engine: as you edit, the app automatically detects and fixes common accessibility problems, recording each one as a reviewable "flag" so a human always signs off on what the machine changed
- Automatic fixes (applied + flagged for your review): decorative emoji removed from lines of text, SHOUTING (all-caps) text converted to normal case, and repeated punctuation (!!!, ???, ...) collapsed — known acronyms (EUSD, PTA, IEP, …) and "do not translate" text are preserved
- "Check & fix" button: runs the full engine on demand, including AI assists — drafts missing image alt text, rewrites bare/vague link text ("click here", raw URLs) into descriptive text, and flags links reused for different destinations; every AI edit is shown for your review and is never auto-accepted
- Must-fix gate items (block publishing until resolved): missing newsletter or section titles, section colors that fail color-contrast against the white title text, links that point to downloadable files, and flyer PDFs that fail the accessibility check (the flyer is always re-checked — a stored "pass" is never trusted)
- Accessibility panel: a per-newsletter summary of all flags with counts (must-fix / to review / auto-fixed / notes), a live-announced status, and an Acknowledge action on each item; counts and new must-fix alerts are announced to screen readers
- Structured flyer extraction: when AI is available, "Extract text from this flyer" organizes the PDF text into separate, ordered draft items for your review (falling back to plain paragraphs otherwise); all extracted and AI-produced content is sanitized before it is stored
- Privacy/safety: AI never runs on the keystroke-autosave path (no typing latency); when AI is unavailable the engine degrades safely — it never silently passes a problem
- Lock integrity: automatic fixes are never written into a section another colleague is editing

## [2026-06-01] Phase 3a complete — Media & AI Assists

- Image and image-grid content blocks: authors can add single images or a side-by-side image grid to any section in the editor
- Flyer (PDF) content block: upload a PDF flyer and embed it inline in the newsletter
- Image upload: EXIF metadata is automatically stripped on ingest; WebP variants (thumbnail, medium, full) are generated alongside the original; assets are stored in a school-scoped directory and are access-gated (only users with school access can retrieve them)
- AI "Generate alt text": suggests descriptive alt text for uploaded images using a vision AI; the suggestion is shown in a review field and must be accepted or edited by the author — never auto-accepted
- Flyer accessibility check: each embedded flyer shows a pass/fail/unchecked accessibility status reusing the smore-ada-checker logic
- "Extract text from this flyer": pulls the PDF's text and any decoded QR-code links into the post body as editable paragraph and link items, so screen-reader users and search engines can access flyer content without relying on the image
- Per-kind autosave validation: the autosave endpoint now validates content per block type before persisting (closes a Phase 2 gap)
- Server-side section-lock enforcement: write requests to a locked section are rejected with 423 at the API layer; the UI advisory lock from Phase 2 is now backed by a server enforcement check
- Security: extracted PDF text is HTML-escaped and QR-code URLs are restricted to http/https to prevent stored XSS
- Accessibility: assertive ARIA live-region announcements for upload failures and save errors; always-on WCAG subset upheld across all new editor controls

## [2026-05-19] Phase 2 complete — Data Model & Editor Core

- Newsletter list page: see all newsletters for your school, create blank, or duplicate the last issue
- Section-based editor: add titled sections and paragraph/link content blocks, drag to reorder sections and blocks within a section
- Autosave: edits are saved automatically as you type; a status indicator confirms saves
- Draft/publish toggle: mark a newsletter published (visible) or back to draft (hidden); publish is state-gated so only drafts can be published
- Section soft-lock: when a colleague has a section open, it shows "Another editor is editing this section" and is read-only for others until the lock expires and the page is reloaded
- Always-on WCAG accessibility: heading hierarchy, ARIA live regions for autosave status, and skip-navigation link applied to all editor and list pages

## [2026-05-15] Phase 1 complete — Foundation & Auth

- Google SSO sign-in implemented: staff authenticate with their @eusd.org Google account; non-EUSD accounts are blocked at the OAuth callback
- School picker landing page live: authenticated users see only the schools their Google Group membership grants access to; anonymous visitors are redirected to sign-in
- Access-denied (403) page: clear message shown when a user tries to reach a school they are not in a Google Group for, with instructions to contact their site administrator
- Logout is POST-only (security hardening against CSRF)
- ProxyFix applied so the app works correctly behind the nginx reverse proxy at newsletters.eusd.org
- Deployment scaffolding added: gunicorn config, systemd service unit, nginx vhost config, and deploy README

## [2026-05-15] Project initialized

- Design spec written: self-hosted Smore replacement with viewership analytics, Google SSO, per-school Google Group access control, WCAG 2.1 AA output
- Phase 1 implementation plan written
- User guide skeleton created at docs/user-guide.html
