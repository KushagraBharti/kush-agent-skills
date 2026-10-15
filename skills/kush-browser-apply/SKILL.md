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

## Local files and helpers

- Resolve the Internship Applications repository root before selecting files. Upload its `Kushagra Bharti Resume.pdf` as the general resume. Current company answers, requested letters, and saved form progress belong in `01_Tailored_Applications/Company Name`. Check `02_Portfolio_and_Visuals` for requested portfolio material and `03_Interview_Prep` for preparation notes. `00_Archive` contains historical work, not the default upload source.
- Use `$kush-application-writer` and the existing `scripts/build-application.sh` when a requested document needs building. For a complete company packet, `./scripts/verify-application.sh --company "Company Name"` checks both named PDFs and renders previews. It requires both resume and cover-letter PDFs; inspect individual uploads directly when a cover letter is not requested.
- The local helpers are macOS shell launchers using Python 3.14. Run them from the repository root or use an absolute launcher path. They create or check files and update the local tracker; they do not fill forms, approve applications, or submit them.
- The dashboard's `scripts/employment-client.mjs` and `backend/src/employment/model.ts` belong to the separate Personal-Site checkout. Locate that checkout and read the existing client; do not create or copy a replacement into this repository's `scripts/` folder.
- After recording confirmed submission through the dashboard client, use the existing `scripts/update-tracker.sh` as described in `$kush-app-workflow` when updating the local workbook. The helper matches by company and defaults to `applied`, so preserve per-posting evidence in the dashboard and pass the intended status explicitly.

The client supports `list`, `publish`, `replace`, `claim`, and `result`. It does not approve applications, control a browser, or wake Codex. Report missing setup without bypassing review.
