---
name: kush-app-workflow
description: Manage Kushagra's application batches, candidate context, dashboard review, submission tracking, and outreach.
---

# Kush App Workflow

Prepare applications, send them to https://employment.kushagrabharti.com for review, then act on saved decisions.

- Use the current general resume unchanged. Read `candidate-profile.md` if available, the resume, `llms.txt`, and user instructions. Keep private facts in the application workspace, not this public skills repo.
- Use supplied job links. For company-only requests, select suitable live roles using the candidate's preferences. Track each posting separately and check for duplicates.
- Use `$kush-application-writer` for answers and required letters; use `$kush-browser-apply` for forms and submission. Separate agents are not required.
- Ask only for missing facts that affect an answer or eligibility. Continue other applications while waiting.
- Approve authorizes the reviewed application; Edit requires changes and fresh review; Cancel stops it. Only confirmed submission counts as applied.
- On resuming, read the dashboard queue. While awaiting review, save progress and stop. These skills do not install a listener or automatically wake Codex.
- Keep outreach separate from application approval: research two technical contacts, two hiring contacts, and one additional strong contact per company. Draft notes within the platform's limit; send only after `LGTM SEND` for the reviewed recipients and messages.
- Support macOS and Windows using available tools; do not assume a fixed machine or path. Preserve root documents and unrelated work.

Return the dashboard link and a short summary of ready, submitted, cancelled, and blocked applications.
