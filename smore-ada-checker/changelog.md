# Changelog

All changes to this project, logged automatically by the Daily User Guide Check.

## 2026-04-07
- Update guide styling — title, label, and changelog formatting
- Backfill changelog — last 30 days of commits
- Replace print button with simple link
- Remove 'Last project activity' line — replaced by automated changelog
- Update user guide — add changelog section and permanent changelog for Thrillshare smore-scan callout


## 2026-03-27
- feat: add timestamps to reports, kill stale servers, increase email note font
- chore: auto-delete reports older than 30 days at startup
- feat: auto-save PDF archive when sending email reports

## 2026-03-26
- Remove dead reference to nonexistent WCAG-CHECKLIST.md
- feat: make opt-in banner toggleable in review page
- fix: duplicate alt text suggestions missing from email, treat Smore placeholder as missing alt text

## 2026-03-23
- fix: update raw URL description in email mockup
- fix: align labels and fix blocks across all three report templates
- fix: label 'Current Link Text' for link issues in PDF report
- Merge feature/html-email-report: HTML email delivery for accessibility reports
- fix: show friendly error when PDF file is locked, with back link
- fix: add CC logging to preview and send routes for debugging
- fix: skip redundant red URL pill for Raw URL issues in email
- feat: make file download fix text editable, match templates
- fix: hide redundant 'How to fix' for file download issues in email
- fix: add 'Back to review' link on preview-sent page
- fix: remove print tip and checkboxes from email â€” Gmail strips @media print
- revert: remove text extent cropping â€” parent width is correct
- fix: crop link screenshots to actual text extent, not container width
- fix: tighten link screenshot crop to parent paragraph width
- chore: widen email report max-width from 800px to 900px
- fix: use fixed 350px width for screenshot column in email
- fix: use spacer td column for Gmail-compatible column gap
- fix: add timestamp to preview email subject to prevent Gmail threading
- fix: convert email layout from flex to table for Gmail compatibility
- fix: Gmail layout issues â€” replace CSS gap with margins throughout
- fix: don't generate PDF during preview, only on Generate Final Report
- fix: handle locked PDF file in preview route gracefully
- feat: support EUSD mail relay (no-auth) alongside Gmail
- feat: add /preview and /send routes for email delivery flow
- feat: add email report HTML generator with action-first layout
- feat: add Notes, To, CC fields and Preview button to review page
- feat: add SMTP and auditor config to .env.example
- feat: add email sender module with SMTP and CID image support

## 2026-03-18
- Fix plan review round 2: headings, current_alt, step numbering
- Fix plan review issues: shutdown, current_text, copy button note, DRY
- Fix spec review issues: duplicate alt text, generic links, input types
- Add HTML email report design spec and mockup
- Improve link checks: independent text/destination checks, Google Workspace detection

## 2026-03-17
- Fix false flyer detection for simple logos and assumption-based flagging
- feat: extract QR code URLs and suggest link text
- Fix false flyer detection for photos with background text
- feat: add step-by-step progress indicator during audit
- Fix flyer detection: thinking tokens, linked images, comparison prompts

## 2026-03-11
- Merge pull request #4 from markberning/provider-label
- feat: show vision provider in filename, review page, and PDF
- Merge pull request #3 from markberning/cleanup-reports
- refactor: save all PDF reports to reports/ folder
- Merge pull request #2 from markberning/review-workflow
- feat: add review workflow with WCAG 2.1 AA compliant review page
- Merge pull request #1 from markberning/unified-vision-provider

## 2026-03-10
- feat: unified vision provider with Claude/Gemini support

## 2026-03-09
- Add README and requirements.txt
- Add .claude/ to .gitignore
- Initial commit: Smore ADA Accessibility Checker

## 2026-04-06
- Added callout linking to Thrillshare Checker smore-scan feature as source for Smore URLs
