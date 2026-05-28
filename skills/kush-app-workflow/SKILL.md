---
name: kush-app-workflow
description: End-to-end internship application workflow for Kushagra that creates company-specific packets, coordinates kush-resume-tailor, kush-cover-letter, and humanizer, answers application writing prompts, builds and visually verifies one-page PDFs, waits for human approval, commits and pushes approved packets, then researches LinkedIn targets, drafts high-quality outreach, and sends only after explicit LGTM SEND approval.
---

# Kush App Workflow

## Purpose

Run the full internship application loop from job posting to approved PDFs, written application answers, Git commit/push, LinkedIn target research, outreach draft review, and approved connection sends.

This skill is procedural. Do not assume personal facts outside the workspace, `llms.txt`, resume, cover letter template, job posting, company research, or explicit user notes.

## Preferred Invocation

```text
Use $humanizer, $kush-resume-tailor, $kush-cover-letter, and $kush-app-workflow.

Company:
<company>

Job posting:
<paste job posting or URL>

Application questions:
<optional writing prompts, limits, or application text boxes>

Extra notes:
<optional positioning, tone, referral context, constraints>
```

When all four skills are invoked, keep using all four throughout the workflow: `$kush-app-workflow` owns sequencing and approval gates, `$kush-resume-tailor` owns resume judgment, `$kush-cover-letter` owns letter judgment, and `$humanizer` owns final prose cleanup for the cover letter, application-question answers, and LinkedIn messages. Do not use `$humanizer` to blur resume facts or technical precision.

## Workspace Contract

Expected files:

```text
llms.txt
resume.tex
Kushagra Bharti Cover Letter - General Template.md
new-tailored-application.cmd
build-application.cmd
03_Tailored_Applications/
```

Hard rules:

- Never edit root `resume.tex`.
- Never edit the root cover letter template.
- Only tailor `03_Tailored_Applications/<Company>/resume.tex` and `cover-letter.md`.
- Put written application answers in `03_Tailored_Applications/<Company>/application-questions.md`.
- Do not create extra reference files unless the user asks or the workflow output requires them.
- If root files change unexpectedly, stop and report before committing.

## Approval Gates

Gate A: resume PDF, cover-letter PDF, and application-question answers are complete. Stop for user feedback or `lgtm`.

Gate B: LinkedIn targets and notes are drafted. Stop for feedback or explicit send approval.

Sending LinkedIn requests requires exactly:

```text
LGTM SEND
```

Plain `lgtm` approves the application packet only; it is not approval to send outreach.

## Workflow

### 1. Intake

1. Identify company, role, job URL/source, application questions, writing limits, and user notes.
2. Inspect the repo layout and confirm required scripts exist.
3. Read `llms.txt`, root `resume.tex`, and root cover letter template.
4. If company or role is ambiguous, ask one concise clarification before creating files.

### 2. Create Company Packet

Run:

```powershell
.\new-tailored-application.cmd "<Company>"
```

If the company folder already exists, inspect it and continue. Do not use `-Force` unless the user explicitly asks.

### 3. Research

Research current company/job facts when a URL, company, or role is externally verifiable. Prefer:

- Job posting page.
- Company careers page.
- Company product, engineering, blog, docs, or about pages.
- LinkedIn company page only for outreach research.

Capture only facts that improve tailoring. Avoid padding the resume or cover letter with vague praise.

### 4. Tailor Resume

Edit only `03_Tailored_Applications/<Company>/resume.tex`.

Light resume guardrails:

- The resume is already full. Improve fit by rewriting, compressing, reordering, and swapping projects.
- Let the job description drive the edits; do not force predefined role buckets or fixed project matrices.
- Build a compact ATS map: `requirement -> evidence -> edit`.
- For internship roles, use `August 2023 -- December 2027` as the education date by default. Use `August 2023 -- May 2027` only for new-grad roles or when the user explicitly asks.
- Add exact job keywords only when supported by `llms.txt`, root resume, company-specific resume, or user notes.
- Preserve project links when known; never invent missing links.
- Project headings should not include dates or status labels.
- Keep the resume one page. If it spills, fix content before formatting.

Use `$kush-resume-tailor` when active; otherwise follow these rules directly.

### 5. Tailor Cover Letter

Edit only `03_Tailored_Applications/<Company>/cover-letter.md`.

Light cover-letter guardrails:

- Start from the company-specific copy of the root cover-letter template and preserve its basic shape unless the user asks otherwise.
- Be brief, specific, and human. Let personality show without sounding corny.
- Connect 2-3 strongest experiences to the role.
- Mention one or two researched company/team/product details.
- Show work ethic and company-values fit through examples, not slogans.
- Keep it one page.

Use this default signature unless the user asks otherwise:

```markdown
Sincerely,
Kushagra Bharti
Student | B.S. Computer Science
The University of Texas at Dallas
[LinkedIn](https://www.linkedin.com/in/kushagra-bharti/) | [Personal Site](https://www.kushagrabharti.com) | [GitHub](https://github.com/KushagraBharti/)
```

In the actual `cover-letter.md`, keep signature lines consecutive with no blank lines inside the signature block, and verify the PDF signature renders cleanly.

Use `$kush-cover-letter` when active; otherwise follow these rules directly.

### 6. Answer Application Questions

If writing prompts are provided:

1. Create or update `03_Tailored_Applications/<Company>/application-questions.md`.
2. Include each prompt, any limit, and a paste-ready answer.
3. Draft after resume and cover letter edits so positioning is consistent.
4. Answer directly and truthfully in Kushagra's voice.
5. Use the answer to address the exact prompt, not to repeat the cover letter.
6. Respect word/character limits. When a limit matters, include a quick count or note for the user.
7. Keep structure flexible; use only enough organization to make the answer clear and easy to paste.

### 7. Build

Run:

```powershell
.\build-application.cmd "<Company>" -Clean
```

Expected outputs:

```text
03_Tailored_Applications/<Company>/Kushagra Bharti Resume - <Company>.pdf
03_Tailored_Applications/<Company>/Kushagra Bharti Cover Letter - <Company>.pdf
```

If the build fails, inspect the LaTeX/Markdown error, fix only company-specific sources, and rebuild.

### 8. Verify

Verify before Gate A:

- Both PDFs exist.
- Resume is one page.
- Cover letter is one page unless the user explicitly wanted longer.
- Render or visually inspect both PDFs.
- Check for clipped text, blank pages, broken links, awkward signature wrapping, placeholder text, checklist text, overlap, wrong company, or wrong role.
- If practical, extract PDF text and confirm key ATS terms survived.
- If `application-questions.md` exists, confirm each prompt has an answer and no placeholders remain.

If visual inspection is not possible, say so clearly and still do file/page checks.

### 9. Gate A Response

Stop and return:

- Resume/cover-letter PDF paths.
- Brief summary of resume changes and cover-letter positioning.
- Compact ATS coverage: covered, partially covered, and unsupported requirements.
- Application-question answers, if any.
- Unsupported requirements or assumptions.
- Verification status.
- Ask for feedback or `lgtm`.

Do not commit, push, research LinkedIn people, or send anything before Gate A approval.

If the user gives feedback, incorporate it and repeat steps 4-9.

### 10. Commit And Push

After Gate A `lgtm`:

1. Run `git status`.
2. Confirm root resume/template were not edited.
3. Verify GitHub remote visibility when possible. If a public repo appears to contain private application materials, stop and ask.
4. Stage only relevant application packet changes.
5. Commit with:

```text
Tailor application for <Company>
```

6. Push the current branch.

### 11. LinkedIn Target Research

After commit/push, use browser automation only if the user is already logged in. Do not bypass captchas, login challenges, anti-automation prompts, rate limits, or access restrictions.

Find 5 high-quality targets. Optimize for conversation quality, not the easiest visible Connect buttons.

Target mix:

- 2 technical targets: engineers, managers, interns, or adjacent team members.
- 2 hiring targets: recruiters, hiring managers, talent partners, university recruiters, or clear people-team contacts.
- 1 free highest-ROI target with no extra restriction.

If the ideal mix is unavailable, choose the strongest substitutes and explain the gap.

Use a light scorecard while choosing: relevance to the role/team, natural conversation hook, sendability, and risk of wasting the request. Inspect profiles before choosing when possible. Prefer people whose background gives a natural reason to ask for insight.

### 12. Draft Outreach

Return each target with:

- Name.
- Role/headline.
- Profile URL.
- Target type.
- Why selected.
- Connection note and character count.

Message rules:

- Keep under 300 characters.
- Aim for 240-300 characters when possible.
- Use straight apostrophes, not curly quotes.
- Be straightforward and normal.
- Make the note naturally invite a reply or short conversation.
- Do not make it a generic connect request.
- Do not claim a relationship, referral, or shared background that is not verified.
- Avoid corny phrasing, inflated admiration, and pitch-like lines.

Default shape:

```text
Hi <Name>, I'm a UTD CS/Finance student applying to <Company>/<Role>. I'm trying to learn/gain experience in <field>, and I'd really appreciate your insight on <specific role/team/topic>.
```

### 13. Gate B Response

Stop after drafting outreach. Ask for feedback or explicit send approval.

If the user gives feedback, revise only targets/messages unless they ask for broader changes.

Do not send connection requests unless the user says exactly:

```text
LGTM SEND
```

### 14. Send Approved Connection Requests

Only after `LGTM SEND`:

1. Send only the approved notes.
2. Do not change the notes except replacing unsupported punctuation with typeable equivalents.
3. Send one person at a time; do not batch multiple invite sends.
4. Never click any path that says or implies sending without a note.
5. Stop if LinkedIn blocks, rate-limits, challenges, rejects the approved under-300-character note, or shows an unexpected prompt.
6. Report sent, skipped, and blocked requests.

Preferred in-app browser method:

1. Open `https://www.linkedin.com/preload/custom-invite/?vanityName=<profile-vanity>`.
2. Inspect the visible modal and click the actual visible `Add a note` button for that modal. Do not reuse coordinates from another profile.
3. Focus the visible `textarea[name="message"]`.
4. Type the normalized note character-by-character with keyboard events. Do not try fill, paste, clipboard writes, or bulk text entry first.
5. Verify all of the following before sending: the note box is visibly open, the full note is visibly present, the counter shows the expected `N/300`, the textarea value exactly matches the approved note, and the Send button is enabled.
6. Click only the verified `Send invitation` button. If any check is ambiguous, stop and report instead of sending.
7. After any timeout, unexpected modal state, or off-target click, inspect the current state before any further click.

## Completion

The workflow is complete when the user-approved application packet is built, verified, committed/pushed, outreach targets/messages have passed Gate B, and approved sends have either completed or been reported as blocked/skipped.
