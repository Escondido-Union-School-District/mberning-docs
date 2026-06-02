# EUSD Newsletters — Changelog

Permanent log of user-facing changes. Never pruned.

---

## [2026-06-02] Phase 6b — React app foundation + new Newsletters Home

- The authoring app is becoming a modern single-page app. The first screen is a redesigned **Newsletters Home**: a visual card grid (each card shows a colored header "cover" with the issue's eyebrow + title), a status badge, and "Issue {date} · Edited {when}".
- One-tap **start row**: "Start a new newsletter" (opens a Blank / Template / Duplicate modal), "Duplicate last issue", and "School analytics".
- **Instant search, status filter pills with live counts, and sort** (last edited / issue date / title) — all client-side, no page reloads.
- Per-card actions: **Edit, Duplicate, Analytics**, and a **⋯ menu** (Rename, Duplicate, View public page [published only], Delete). Deleting shows a 5-second **Undo** toast — the newsletter isn't actually removed on the server until the undo window passes.
- Accessibility built in: keyboard-accessible menus and modals with proper focus handling (via an accessible component library), labeled search/filters/sort, status conveyed with text (not color alone), and reduced-motion support. (Authoring screens now require JavaScript; the public newsletter pages stay server-rendered and work without it.)
- Under the hood: a React + TypeScript app (built with Vite) served by Flask, talking to new JSON APIs for the newsletter list, create, duplicate, rename, and delete. The **public newsletter pages and the accessibility/publish gate are unchanged** and remain the canonical, server-rendered output. "Edit" still opens the current editor for now; the new builder arrives next (6c).
- 409 server tests + 12 frontend tests passing. No new server dependencies. (Run `cd frontend && npm install && npm run build` to build the app bundle.)

## [2026-06-02] Phase 6a — Builder backend foundation (new elements, branding, document API)

- Newsletters can now contain five new content elements, rendered on the public page: a **Title/heading** (large/medium/small, keeping a correct heading order), a **rich Text block** (a safe markdown-lite: bold, italic, links, bullets, paragraphs), an **Event** (title + date/time/location), a **Quote**, and a **Divider**. The original paragraph, link, image, image grid, and flyer elements are unchanged.
- New per-school **Branding settings page** (`/app/<school>/settings/branding`): set the school's banner color and accent color. The page checks contrast live — the banner is always readable (white title on a darkened scrim over any color), and the accent color is **blocked from saving unless it meets WCAG AA 4.5:1**, with the failing ratio and a darker suggested color shown.
- Groundwork for the new live-preview builder (coming in 6b): a JSON document API now loads and saves a whole newsletter in one call. It validates every element, respects section locks, runs the accessibility engine, and reverts a published newsletter to draft on any edit — same safety rules as the existing editor, which still works exactly as before.
- Security: the new Text block is sanitized on the server (escaped first, links scheme-allowlisted) — author input is never trusted as raw HTML.
- Accessibility: the new elements use semantic markup (headings, definition lists, blockquote/figure, decorative dividers hidden from screen readers); the Branding page has labeled fields, an announced error, and conveys pass/fail with text, not color alone.
- 396 tests passing. No new third-party dependencies.

## [2026-06-02] Phase 5b complete — Analytics Dashboards & Rollup (Phase 5 done)

- Analytics dashboards are live: open "View analytics for this newsletter" from the editor, or "View analytics for <school>" from the newsletter list.
- Per-newsletter dashboard: total views, unique daily visitors, reading depth (how many readers reached 25/50/75/100%), average time on page, device (mobile/tablet/desktop), where readers came from (email/website/social/direct), approximate location (city/region), and your most-clicked links & flyers.
- Per-school dashboard: combined totals across all the school's newsletters, plus a most-viewed leaderboard linking to each newsletter's own dashboard.
- Today's figures are live; earlier days are finalized once a night by a scheduled rollup job (which also trims raw event data past the retention window — the daily summaries are kept).
- Privacy unchanged and intact: no cookies, no cross-day or cross-site tracking; "unique visitors" means unique-per-day; no IP address, browser string, or student data is ever stored.
- Accessibility: dashboards are plain, screen-reader-friendly data tables (captioned, proper row/column headers), work with JavaScript off, and never rely on color alone.
- Approximate location only appears once the server has the optional location database installed; otherwise that table stays blank and nothing else is affected.
- 343 tests passing. This completes Phase 5 — and all five planned build phases.

## [2026-06-02] Phase 5a complete — Analytics Capture (privacy-scrubbed)

- Viewership capture: published newsletters now quietly record how they're read — page views and unique daily visitors, which sections get seen, how far people scroll (25/50/75/100%), time on page, and which links and flyers get clicked. (The dashboards that show all this are coming next, in Phase 5b.)
- Privacy by design (FERPA/COPPA posture): no cookies and no tracking across days or sites. A visitor is counted using a one-way, salted code that is rebuilt fresh each day from the visit details and cannot be reversed — so "unique visitors" means unique-per-day and no one is followed over time.
- No personal data is ever stored: the visitor's IP address and browser are used for a split second to derive a coarse device type (mobile/tablet/desktop) and an approximate city/region, then immediately discarded — they are never written to the database. A built-in test proves a raw IP or browser string never lands in any stored field.
- Counting only happens for published newsletters; an unpublished address records nothing and still returns "not found." The measurement script is optional enhancement — the public page works exactly the same with it switched off.
- Geography is best-effort: city/region appear only when a local lookup database is installed on the server; without it, those fields simply stay blank — nothing breaks.
- 321 tests passing.

## [2026-06-02] Phase 4b complete — Review Workflow & Collaboration

- Status state machine widened: newsletters now move through draft → in_review → changes_requested → approved → published (with submitted/reviewed audit columns and a review note); a new migration adds the columns.
- Submit-for-review gate: an author can only submit a newsletter once it passes the full accessibility engine (the same run as Publish, including the flyer-PDF re-check) and is_publishable — no unresolved must-fix items and no unacknowledged machine edits.
- Per-school approver group + can_approve: approvers are resolved live from the school's approver Google Group; a district platform admin can always approve as a fallback. You cannot approve your own newsletter (no self-approval).
- Approve / Request-changes routes: an approver either approves (clearing the newsletter for publishing) or requests changes with a required note back to the author, returning it to draft.
- Reviewer screen: a read-only render of the public page exactly as readers will see it, alongside an audit list of every ContentFlag (accessibility change) and who acknowledged it, with Approve / Request-changes actions.
- Status-aware editor gate buttons: Submit / Approve / Request-changes / Publish appear based on the current status and the gate result; a blocked button is disabled and explains why.
- Edit-reverts-to-draft: editing an approved or published newsletter automatically reverts it to draft so it must go through review again — approval applies to the exact content reviewed.
- Accessibility: the new reviewer screen and gate buttons uphold the always-on WCAG 2.1 AA subset.
- 291 tests passing.

## [2026-06-02] Phase 4a complete — Public Renderer, Reader Controls & Publish Gate

- Public newsletter page: every published newsletter now has a stable public web address families can open on any device — no sign-in required. Drafts stay private (an unpublished address returns "not found").
- Smore-faithful design: bold colored hero, colored section headers, photo-forward layout, mobile-first responsive (image floats and grids reflow on small screens).
- Reader-controls toolbar on every published page: jump-to-section table of contents (works even with JavaScript off), text size (small/medium/large), high-contrast mode, on-demand translation, and a Share button — all keyboard accessible. Your text-size and contrast choices are remembered on your device only (no cookies, nothing sent to a server).
- Image lightbox: click or press Enter on any photo to view it larger, with full keyboard support (arrow keys between gallery images, Esc to close) and reduced-motion respected.
- Accessible flyer cards and links: flyers render as a clear download card (file type indicated, opens in a new tab announced to screen readers); links use their descriptive text.
- Link previews: Open Graph / Twitter Card tags so a pasted newsletter link unfurls with its title and school.
- Security: author-typed text is sanitized before it appears on the public page (only bold, italic, and safe web/email links survive) so a crafted draft can never inject scripts; links with unsafe addresses are neutralized; images are served only when they belong to that published newsletter.
- Publish gate: publishing now re-runs the full accessibility engine as a final check — including re-verifying every flyer PDF (a stored "pass" is never trusted) — and is blocked until all must-fix items are resolved and every machine edit is acknowledged.
- Preview: editors can preview the exact public page for a draft before publishing.

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
