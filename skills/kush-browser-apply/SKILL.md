---
name: kush-browser-apply
description: Fill application forms, publish form readbacks for review, and submit dashboard-approved applications with confirmation.
---

# Kush Browser Apply

Use available browser/computer tools and the user's chosen browser. Follow the tools' interaction and confirmation rules.

- Fill the actual form using confirmed candidate facts and `$kush-application-writer` answers. Upload the approved general resume unchanged; omit optional cover letters unless requested. Verify parsed fields and uploads.
- Use `scripts/employment-client.mjs` in the Personal-Site checkout. Read its usage and `backend/src/employment/model.ts` for the current protocol. Use the configured `EMPLOYMENT_WORKER_TOKEN`, never reviewer or service-role credentials.
- Publish actual readbacks: all submitted fields and choices, full answers, document names and SHA-256 hashes, cover-letter text, and blockers. Exclude passwords, cookies, tokens, and one-time codes. Keep private working files out of Git.
- Save application IDs and enough progress to resume. Flag missing facts or browser blockers and continue independent applications.
- On Edit, apply feedback and publish a replacement for fresh review. On Cancel, stop.
- Before Submit, read the latest approval, re-read the live form, and successfully claim the exact approved version. Changed answers, documents, or URLs require fresh approval. Dashboard approval does not override browser-tool confirmation rules.
- Record the site's actual submission confirmation. Interrupted or ambiguous attempts are uncertain; inspect them before proceeding and never automatically repeat Submit.

The client supports `list`, `publish`, `replace`, `claim`, and `result`. It does not approve applications, control a browser, or wake Codex. Report missing setup without bypassing review.
