---
name: kush-app-workflow
description: End-to-end internship application workflow for creating company-specific application packets, coordinating kush-resume-tailor, kush-cover-letter, and humanizer when explicitly invoked, building and visually verifying PDFs, waiting for human approval, committing/pushing approved changes, then researching LinkedIn connections and drafting/sending approved outreach notes. Use when the user asks to apply to a company, tailor an application, build application PDFs, or run the Kush application workflow.
---

# Kush App Workflow

## Purpose

Run a repeatable internship application workflow from a company/job posting through tailored PDFs, human review, commit/push, LinkedIn connection research, message drafting, and approved connection sends.

This skill is procedural only. Do not assume any personal facts that are not present in the user's workspace, resume, cover letter template, `llms.txt`, or explicit prompt.

## Preferred Invocation

The intended full prompt explicitly loads all companion skills:

```text
Use $humanizer, $kush-resume-tailor, $kush-cover-letter, and $kush-app-workflow.

Company:
<company>

Job posting:
<paste job posting or URL>

Extra notes:
<optional>
```

If `$kush-resume-tailor` is active, use it for Step 4. If `$kush-cover-letter` is active, use it for Step 5. If `$humanizer` is active, use it only as a final prose polish for the cover letter and LinkedIn messages; do not use it to change resume facts or technical precision.

## Expected Inputs

Accept:

- Company name, if provided.
- Exact job posting text or URL.
- Optional user notes about positioning, tone, target team, referral context, or constraints.

If the company name is missing, infer it from the job posting only when unambiguous. If the role or company is ambiguous, ask one short clarification before creating files.

## Workspace Assumptions

Run from the user's application-materials repository. Prefer discovering paths from the current working directory, but the workflow expects:

```text
llms.txt
resume.tex
Kushagra Bharti Cover Letter - General Template.md
new-tailored-application.cmd
build-application.cmd
03_Tailored_Applications/
```

Hard rule: never edit the root `resume.tex` or root cover letter template. Only edit the company-specific copies inside:

```text
03_Tailored_Applications/<Company>/resume.tex
03_Tailored_Applications/<Company>/cover-letter.md
```

If root files change unexpectedly, stop and report before committing.

## Workflow

### 1. Intake

1. Read the user prompt.
2. Identify company, role, job posting source, and any user-provided positioning notes.
3. Inspect the repo layout and confirm the required scripts exist.
4. Read `llms.txt`, root `resume.tex`, and the root cover letter template for context.

### 2. Create Company Packet

Run:

```powershell
.\new-tailored-application.cmd "<Company>"
```

If the company folder already exists, do not use `-Force` unless the user explicitly asks. Inspect existing company files and continue from them.

### 3. Research

Research the company and job posting online when the posting URL or company is current, niche, or externally verifiable. Prefer primary sources:

- Job posting page.
- Company careers page.
- Company product/engineering/blog pages.
- LinkedIn company page only for outreach research.

Capture only facts useful for tailoring. Do not pad the resume or cover letter with vague company praise.

### 4. Tailor Resume

Edit only the company-specific `resume.tex`.

Use `$kush-resume-tailor` when it is active. If it is not active, follow the rules below directly.

Make minimal, truthful ATS-focused changes:

- Reorder or lightly rewrite bullets to emphasize job-relevant experience.
- Add job keywords only when supported by existing experience.
- Prefer concrete skills, systems, metrics, tools, and outcomes.
- Preserve all factual claims, dates, titles, schools, roles, and metrics unless the source materials support the change.
- Keep the resume to one page.
- Avoid broad rewrites that change the user's voice or positioning without a reason.

Never invent experience, credentials, awards, tools, employment, metrics, or education.

### 5. Tailor Cover Letter

Edit only the company-specific `cover-letter.md`.

Use `$kush-cover-letter` when it is active. If it is not active, follow the rules below directly.

Use the cover letter to make stronger company-specific changes:

- Replace placeholders.
- Remove checklist/template-only sections.
- Connect the role to 2-3 strongest matching experiences.
- Include one or two specific company/product/team details from research.
- Keep the tone direct, human, and specific.
- If `$humanizer` is active, use it to polish the final cover letter after the factual draft is complete. If no humanizer skill is active, do normal professional editing without claiming a humanizer pass.

### 6. Build PDFs

Run:

```powershell
.\build-application.cmd "<Company>" -Clean
```

The expected outputs are:

```text
03_Tailored_Applications/<Company>/Kushagra Bharti Resume - <Company>.pdf
03_Tailored_Applications/<Company>/Kushagra Bharti Cover Letter - <Company>.pdf
```

If the build fails, inspect the LaTeX/Markdown error, fix the company-specific source, and rebuild.

### 7. Verify PDFs

Verify both PDFs before asking for feedback:

- Confirm each file exists.
- Confirm the resume is one page.
- Confirm the cover letter is one page unless the user explicitly wants longer.
- Render or visually inspect the PDFs when the environment supports it.
- Check for obvious layout failures: clipped text, blank pages, broken links, placeholder text, checklist text, overlapping content, or incorrect company/role names.

If visual inspection is not possible, say so clearly and still perform file/page-count checks.

### 8. Human Review Gate A

Stop and return:

- Summary of what changed.
- Paths to the generated PDFs.
- Any known risks or unresolved assumptions.
- Ask the user to provide feedback or say `lgtm`.

Do not commit, push, research LinkedIn people, or send anything before this approval.

If the user gives feedback, incorporate it and repeat steps 4-8.

If the user says `lgtm`, continue to commit and push.

### 9. Commit And Push

Before committing:

1. Run `git status`.
2. Confirm root `resume.tex` and root cover letter template were not edited.
3. If the application repo has a GitHub remote, verify visibility before pushing when possible. If the repo appears public and contains private application materials, stop and ask.

Commit only relevant application packet changes. Use a clear message:

```text
Tailor application for <Company>
```

Push after a successful commit.

### 10. LinkedIn Research

After the application packet is approved and pushed, use browser automation only if available and the user is already logged in.

Do not bypass captchas, login challenges, anti-automation prompts, rate limits, or access restrictions. If LinkedIn blocks or challenges the session, stop and ask the user to take over.

Research:

1. Open LinkedIn.
2. Search for the company page.
3. Go to People.
4. Identify 5 strong connection targets.

Prioritize:

- University alumni.
- Recruiters or university recruiters.
- Engineers or managers near the target role/team.
- Mutual connections.
- People with recent activity or a plausible reason to connect.

### 11. Draft Outreach

For each of 5 people, return:

- Name.
- Role/headline.
- LinkedIn profile URL.
- Why this person was selected.
- A personalized connection note under 300 characters.

Connection-note rules:

- Keep each note under 300 characters.
- Be specific and low-pressure.
- Mention the role/company context only if natural.
- Do not claim a relationship, referral, or shared background that is not verified.
- Do not ask for too much in the first note.
- If `$humanizer` is active, use it to polish the notes while preserving the 300-character limit and all factual claims.

### 12. Human Review Gate B

Stop and ask the user for feedback or explicit send approval.

If the user gives feedback, revise only the outreach messages and return them again.

Do not send connection requests unless the user explicitly says:

```text
LGTM SEND
```

Plain `lgtm` is not enough for sending LinkedIn requests.

### 13. Send Approved Connection Requests

Only after `LGTM SEND`:

1. Use browser automation to open each profile.
2. Send a connection request with the approved note.
3. Do not change the note without asking.
4. Stop if LinkedIn blocks, rate-limits, or shows unexpected prompts.
5. Report which requests were sent and which were not.

## Completion

The workflow is complete only after either:

- The tailored application packet has been built, verified, and returned for review; or
- The user has approved the packet, changes have been committed/pushed, LinkedIn targets/messages have been reviewed, and explicitly approved connection requests have been sent.
