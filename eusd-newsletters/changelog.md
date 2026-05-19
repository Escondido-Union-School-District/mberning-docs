# EUSD Newsletters — Changelog

Permanent log of user-facing changes. Never pruned.

---

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
