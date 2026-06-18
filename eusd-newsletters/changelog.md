# EUSD Newsletters — Changelog

Permanent log of user-facing changes. Never pruned.

---

## [2026-06-18] Analytics redesign, read-to-end fix, inline link tracking, mobile filters, login chooser

- **Unique visitors as the hero metric:** Analytics dashboards (per-newsletter and per-school) now lead with unique visitors in a highlighted card. Total views moves to a muted context line beneath it ("N total views · X per visitor"). The device, network, location, and referral breakdowns now count unique visitors per bucket rather than total page loads.
- **Read-to-end % fixed:** The "Read to the end" KPI now divides by unique visitors rather than total views. One visitor who finished the newsletter across 4 page loads now shows 100%, not 25%.
- **Inline link click tracking:** The "most-clicked links" table now includes inline links inside newsletter body text (including URLs extracted from flyer images via OCR or QR codes), not just dedicated Link and Flyer elements.
- **Mobile status filter dropdown:** Status filters on the Newsletters Home are now a compact dropdown on mobile instead of a row of crowded buttons.
- **Duplicate last issue names the newsletter:** The "Duplicate last issue" button now shows the title of the newsletter it will copy, with a tip to use the ... menu to duplicate any specific issue.
- **Login account chooser:** Google sign-in now always shows the account chooser scoped to @eusd.org, so users can select their EUSD work account even when a personal account is already active on the device.

---

## [2026-06-05] Secondary page-background color, banner image upload, school-color art variants, per-newsletter logo size, home card header text

- **Secondary color (page background):** New optional branding field on the School Branding screen. Sets a tint for the page background behind newsletter content cards. Left blank = plain white. Not contrast-gated (it sits behind white cards, not text). Any valid hex color is accepted.
- **Banner image upload:** Authors can now upload a photo as the newsletter banner background directly in the Header editor. The uploaded image takes precedence over art presets and shows on both the live preview and the published page. Reuses the existing photo-upload pipeline (strips EXIF; text-heavy flyer images are rejected).
- **School-color art preset variants:** Each of the 6 illustrated banner presets (Waves, Nature, Floral, Fireworks, Confetti, Sunrise) now has a school-color variant (`<key>-brand`) that applies the preset's overlay texture over a gradient built from the school's `brand_primary`. Variants appear in the picker only when the school has a primary color set.
- **Per-newsletter logo size:** When the school logo is enabled in the banner, a Small / Medium / Large size control is now available per newsletter. The chosen size is reflected in the live preview and the published page.
- **Home cards show actual header text:** Newsletter cards on the Newsletters Home now display the author's actual banner header text instead of the internal template name (e.g., "Weekly Update"). Both the decorative cover title and the body editor link reflect the real banner headline.
- **Builder polish:** New newsletters now pre-fill the Header editor's "Header text" field from the title chosen at creation time. Editing a published newsletter shows a dismissable inline status banner rather than a modal popup.

---

## [2026-06-04] Per-school branding on the published newsletter page

- The public newsletter page now renders the school's identity in the hero header.
- **School logo** appears in the banner when one has been uploaded under School Branding.
- **Banner color** uses the school's saved brand primary color (falls back to EUSD blue `#1031A3` if none is set). The author's per-newsletter hero color override still wins when set.
- **Accent color** (the school's saved brand accent) paints body content links on the published page. The accent is only accepted at save time if it passes WCAG AA (4.5:1 on white), so links are always readable.
- A dark scrim overlay is applied unconditionally so the white newsletter title clears WCAG AA contrast over any banner color.
- No action needed from authors; the public page reflects School Branding settings automatically on every published newsletter.

## [2026-06-04] School-agnostic empty-state icon

- The empty newsletter list (no newsletters yet, or no search results) and the empty builder preview previously showed a 🦦 otter emoji — a leftover from the Oak Hill Otters test school used during development.
- Both are now replaced with a neutral document icon styled in the app's brand color.

## [2026-06-03] Phase 6e₃ — The reviewer screen, now in the app

- **The reviewer screen moved into the app** (the same React app as the builder and dashboards). Open it from a newsletter's **Review** action.
- It shows everything an approver needs in one place: a link to the **read-only public preview** (the page exactly as families will see it), the **previous reviewer note**, and the full **accessibility audit** — every finding with its rule, location, the **before/after** of any automatic fix, and **who acknowledged it and when**.
- Approvers get **Approve** and **Request changes** right there (Request changes opens an accessible dialog for the required note, addressed to the author by name). The same safety rules apply as everywhere else — no self-approval, approver-group enforced on the server.
- This was the **last server-rendered authoring screen** — the entire authoring app (Home, builder, analytics, branding, review) is now a single accessible React app. The public newsletter pages families read stay server-rendered and canonical by design.
- 511 server + 153 frontend tests passing. No new dependencies. (Run `cd frontend && npm install && npm run build` to build the app bundle.)

## [2026-06-03] Phase 6e₂ — School branding & logo, now in the app

- **Set your school's branding inside the app** (the same React app as the builder), no separate page reload. Choose a **banner color** (used behind the newsletter header) and an **accent color** (buttons and links).
- **Live accessibility check.** As you type an accent color, the screen shows its contrast against white in real time. The accent **can't be saved until it meets WCAG AA (4.5:1)** — and if it falls short, one click applies a suggested darker shade that passes. (The check is enforced on the server too, so it can't be bypassed.)
- **Upload a school logo** (image only) — shown right away in a **live header preview** alongside your chosen colors, so you can see how the newsletter banner will look. Remove or replace it any time.
- Fully accessible: every field is labelled, the contrast result and save confirmation are announced to screen readers, the failing-contrast warning is conveyed by text (not color), and the logo uploader and "use a darker shade" suggestion are keyboard-operable.
- 507 server + 146 frontend tests passing. No new dependencies. (Run `cd frontend && npm install && npm run build` to build the app bundle.)

## [2026-06-03] Phase 6e₁ — Analytics, now inside the app

- **The analytics dashboards are now part of the in-app experience** (the same React app as the builder), so they load instantly and update as you change the time range — no page reloads. Both the per-school dashboard ("How {school}'s newsletters are doing") and the per-newsletter dashboard moved into the app.
- **Pick your time range** — **7, 30, or 90 days** — with a single toggle; today's figures update live.
- **Four headline figures**, each with a "vs previous period" trend: **Total views**, **Unique visitors (daily)**, **Avg. time on page**, and **Read to the end** (the share of readers who reached the bottom of the newsletter). *(The mockup's "Subscribed families" card was replaced with "Read to the end" — this platform doesn't send email or track subscribers, so we show a real engagement number instead of one we can't measure.)*
- **Daily views** bar chart (today highlighted), a **By device** breakdown (mobile / desktop / tablet with counts and shares), the **Newsletters by views** leaderboard (school view), and **Most-clicked links & flyers** (newsletter view).
- **Fully accessible charts:** every chart carries the same numbers as real text — a screen-reader data table or a labelled legend — so nothing depends on seeing color, and each chart has a spoken summary. The time-range toggle, trend arrows, and links all work by keyboard and read correctly to assistive technology.
- 500 server + 132 frontend tests passing. No new dependencies. (Run `cd frontend && npm install && npm run build` to build the app bundle.)

## [2026-06-03] Phase 6d₂ — Upload a flyer, get accessible text automatically

- **Upload a finished flyer and the app lays it out for you.** Choose **Flyer → Upload a flyer** and upload an **image** of a flyer (no PDFs). The app reads it — text *and* layout — and builds a **two-column block**: the flyer image on one side (click to enlarge) and the flyer's words on the other, rendered as **real headings, lists, links, and paragraphs**. A raw flyer picture is invisible to screen readers; this turns it into structured, screen-reader-friendly text beside the image — the accessible page families actually read.
- **QR codes become real, descriptive links** — never a bare web address — so the flyer's "scan me" actions work for everyone.
- **You review and edit before publishing.** The extracted title, image description, and text are drafts you can adjust; pick the **layout** — image on the **left**, **right**, or **top**. (AI assists; you confirm.)
- **Bilingual flyers.** If a flyer comes in English and Spanish, add the **Spanish version** as an optional second image. Both flyers are shown side by side (each click-to-enlarge, the Spanish one labeled), and the reader's translate button handles the words — so you add only the picture, not a second block of text.
- **Accessibility gate.** An uploaded flyer can't be published until its image (and any Spanish companion image) has a description. Decorative chrome is hidden from screen readers, the image opens in a focus-trapped lightbox (keyboard + arrow-key navigation between the two flyers), and every text/background pair meets WCAG AA contrast.
- 483 server tests + 118 frontend tests passing. No new dependencies. (Run `cd frontend && npm install && npm run build` to build the app bundle.)

## [2026-06-03] Phase 6d — Photo uploads + seven accessible flyer styles

- **Upload photos right in the builder.** Pick an image for an Image element, an Image grid, or a photo flyer; the file is validated, EXIF-stripped, and stored as web-optimized variants. Image upload is **for photos only** — if you try to upload a text-heavy *flyer* image, the builder recognizes it and points you to the **Flyer** element instead (the dedicated, accessible way to publish event flyers).
- **Seven flyer styles.** A flyer is no longer a flat PDF link — choose from seven designed "callout" layouts: **Classic, Calendar tear-off, Spotlight split, Ticket stub, Poster hero, Polaroid pinboard,** and **Save-the-date ribbon.** A style picker shows each option in the live preview as you build.
- **Flyers now render as real, accessible HTML** on the public page (not an image of a flyer): the title is a proper heading, the date/time/location are real selectable text, decorative chrome (rings, perforations, tape) is hidden from screen readers, and every text/background pair meets WCAG AA contrast (verified per style; photo styles force a dark legibility scrim so text stays readable over any image).
- **Publish gate for photo flyers.** A photo-style flyer with a photo but **no alt text** is blocked from publishing until you describe the image — the same accessibility gate that protects every other element.
- **Coming next (6d₂):** upload a finished flyer image and have it auto-laid-out (text extracted and reflowed into an accessible two-column design).
- 445 server tests + 89 frontend tests passing. No new dependencies. (Run `cd frontend && npm install && npm run build` to build the app bundle.)

## [2026-06-03] Phase 6c — The live newsletter builder is here

- **The live newsletter builder is here.** Clicking **Edit** (or creating a newsletter) now opens a brand-new two-pane builder: you compose sections and elements on the left and watch a **live preview** of the finished newsletter update on the right as you type — no "save and reload" round-trip.
- **Build with sections and elements.** Each newsletter is organized into sections; add a Title, Text (with **bold/italic/links/bullets**), Button/Link, Event, Quote, or Divider to any section. Edit the **banner** (header text, eyebrow, date, and a background look) right at the top.
- **Reorder however you like** — drag an element or section with the mouse, **or** use the keyboard-accessible **Move up / Move down** buttons (no mouse required). Duplicate or delete any element/section, with a 5-second **Undo** toast after a delete.
- **Autosave.** Your work saves automatically in the background; a status pill reads "Saving…" then "All changes saved." There's no Save button to remember.
- **Submit for review** straight from the builder: an accessible dialog with an optional note for your reviewer. The same safety gate applies — submitting re-runs the **full accessibility check** and is blocked (with a clear message) until the newsletter passes.
- Accessibility: exactly one page heading (the preview banner), proper header/main landmarks, every field labeled, the size/style/device pickers are real radio/toggle groups, the save status is announced politely, and links in the preview warn that they open in a new tab. Reduced-motion is honored.
- The old form-based editor has been **retired** — the React builder fully replaces it. (The public newsletter pages stay server-rendered and remain the canonical, accessibility-gated output.)
- **Coming next (6d):** uploading photos, image grids, and flyer PDFs from inside the builder, plus the rich flyer styles. 343 server tests + 73 frontend tests passing. No new dependencies. (Run `cd frontend && npm install && npm run build` to build the app bundle.)

## [2026-06-03] Phase 6b₂ — Approver home & review workflow on the landing page

- The **Newsletters Home is now role-aware**. Approvers (a school's approver group, or a platform admin) get a **Staff / Admin** toggle in the header; everyone else just sees the staff view. Switching to **Admin** retitles the page "Newsletters to review" and surfaces a **"N newsletters waiting for your approval"** banner that jumps straight to the review queue.
- Each card now shows the **author** (name + initials chip), and search matches **title or author**.
- A per-card **workflow strip** shows where each newsletter stands and offers the right next step: a draft can be **Submitted for approval**; an in-review newsletter shows "Awaiting approval" and (for an approver) **Approve** / **Request changes**; a newsletter with changes requested shows the **reviewer's note** and a **Resubmit** button; an **approved** newsletter offers **Publish**; a published one shows who approved it. (Scheduling a send is coming in a later phase — for now "approved" goes straight to Publish.)
- **Request changes** opens an accessible dialog with a "What needs changing?" note field; the note is required and travels back to the author.
- The same safety gates apply as everywhere else: submitting and publishing re-run the **full accessibility check** and are blocked (with a clear toast) until the newsletter passes; approving enforces **no self-approval** and approver-group membership on the server — the UI only hides buttons as a courtesy.
- Accessibility: status is conveyed by **text, not color**; the toggle and banner are real labeled buttons; the dialog traps focus and has a labeled field; the tinted workflow strips and the coral "Send back" button meet WCAG AA contrast.
- 417 server tests + 19 frontend tests passing. No new dependencies.

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
