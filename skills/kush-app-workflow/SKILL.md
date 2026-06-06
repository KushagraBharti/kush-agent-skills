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
scripts/new-tailored-application.cmd
scripts/build-application.cmd
Application_Tracker.xlsx
03_Tailored_Applications/
```

Hard rules:

- Never edit root `resume.tex`.
- Never edit the root cover letter template.
- Only tailor `03_Tailored_Applications/<Company>/resume.tex` and `cover-letter.md`.
- Put written application answers in `03_Tailored_Applications/<Company>/application-questions.md`.
- Do not create extra reference files unless the user asks or the workflow output requires them.
- If updating the application tracker, keep the simple schema exactly as-is unless the user asks for a new column.
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
5. If multiple postings for the same company are supplied or detected together, treat them as one company packet unless the user explicitly asks for separate packets. Tailor the resume toward the combined requirements across those postings instead of overfitting to one job description.

On Windows, `llms.txt` may be a symlink whose file length appears as `0`. Do not check size/length to decide whether it has content. Read it directly with:

```powershell
Get-Content -LiteralPath .\llms.txt
```

### 2. Create Company Packet

Run:

```powershell
.\scripts\new-tailored-application.cmd "<Company>"
```

If the company folder already exists, overwrite and refresh the workflow-generated packet files for the current posting or posting set:

```powershell
.\scripts\new-tailored-application.cmd "<Company>" -Force
```

Existing company-specific `resume.tex`, `cover-letter.md`, generated PDFs, job metadata, and workflow artifacts may be replaced. Do not revert or touch unrelated files outside the target company folder unless the workflow explicitly requires a shared tracker update.

### 3. Research

Research current company/job facts when a URL, company, or role is externally verifiable. Use your web search capabilities. Prefer:

- Job posting page.
- Company careers page.
- Company product, engineering, blog, docs, or about pages.
- LinkedIn company page only for outreach research.

If the job posting is supplied as a URL/link, use the [$browser:control-in-app-browser](C:\\Users\\kushagra\\.codex\\plugins\\cache\\openai-bundled\\browser\\26.527.31326\\skills\\control-in-app-browser\\SKILL.md) skill to open and read the posting in detail before tailoring. If the user supplied only pasted job text and no link, browser inspection is not required for the posting. If the linked posting is inaccessible, blocked, expired, or requires login, report that clearly and use the best available job text or search result instead.

Capture only facts that improve tailoring. Avoid padding the resume or cover letter with vague praise.

### 4. Tailor Resume

Edit only `03_Tailored_Applications/<Company>/resume.tex`.

Always use `$kush-resume-tailor`. It is necessary.

Resume tailoring is conservative by default. Kushagra's base resume is already strong and full; the goal is to make the existing page read slightly closer to the posting, not rebuild it.

Common rules:

- Let the job description drive small edits, but assume most postings need only minimal keyword and emphasis changes.
- If tailoring for multiple postings at the same company, build one combined ATS map across the postings and optimize for the strongest shared requirements. Do not create separate resumes for each posting unless the user explicitly asks.
- Build a compact ATS map before editing: `requirement -> supported evidence -> minimal edit`.
- Preserve the resume's density. Do not delete bullets or shrink the resume into a visibly underfilled page just because some bullets are less directly relevant.
- Do not edit the LaTeX preamble, custom commands, header/contact block, spacing commands, section structure, or link syntax.
- Add exact job keywords only when supported by `llms.txt`, root resume, company-specific resume, or user notes.
- Keep the resume one full page. If it spills or becomes underfilled, fix content first and preserve density before touching formatting.

Education rules:

- The only normal edit above Experience is the Education date.
- For internship roles, set Education to `August 2023 -- December 2027`.
- For new-grad roles, set Education to `August 2023 -- May 2027`.
- Do not leave the base `May/Dec 2027` placeholder.
- Do not change the school, degree, minor, certificate, coursework, or location unless the user explicitly asks.

Technical Skills rules:

- Treat Technical Skills as no-touch by default.
- Only add one or two supported skills if the posting explicitly values them, the evidence exists in `llms.txt` or the resume, and the addition does not require removing or reordering existing skills.
- Never remove, reorder, rename, or broadly tune skills for ATS.

Experience rules:

- Do not swap, remove, or reorder experiences. Experience chronology is fixed.
- Preserve each experience's title, employer, date range, location, and link.
- Do not change the number of experience bullets by default. Rewrite bullets in place. Rewrites should preserve high quality and density, foreground supported keywords, and maintain ATS coverage. Rewrites should be clear and maintain the same voice and writing style as the rest of the resume.
- Do not broadly compress experience content. If a bullet is lower-fit, usually rewrite it to foreground supported job language instead of cutting it.

Project rules:

- Preserve project links when known; never invent missing links.
- Keep project-heading right-side date/status arguments empty.
- Preserve project order and bullet count unless a rare project swap truly requires adjustment.
- Swap a project only in rare, high-signal cases where the posting has a clear domain match that the current projects miss, such as RL, LLM agents, computer vision, web-dev, quant, backend systems, etc. This should be pretty rare, and it's your judgement. You can either swap a project cleanly, or reduce the existing projects to make space for a new one. Either way it should be rare. The existing 3 projects are already very strong contenders.
- When a rare swap is justified, replace or shorten the lowest-fit current project. Do not append extra projects. Make the judgment call based on the role, the posting, and the strongest evidence in `llms.txt`.

Final checks:

- The resume should still look like a full, dense, high-signal one-page resume.
- The edit should read as targeted tailoring, not a reconstructed resume.
- Unsupported requirements should be reported, not forced into the resume.

### 5. Tailor Cover Letter

Edit only `03_Tailored_Applications/<Company>/cover-letter.md`.

Light cover-letter guardrails:

- Start from the company-specific copy of the root cover-letter template and preserve its basic shape unless the user asks otherwise.
- Be brief, specific, and human. Let personality show without sounding corny.
- Connect 2-3 strongest experiences to the role.
- Mention one or two researched company/team/product details.
- Show work ethic and company-values fit through examples, not slogans.
- Omit unsupported technologies entirely; do not write "I do not have experience with X" disclaimers.
- Keep it one page maximum.

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
.\scripts\build-application.cmd "<Company>" -Clean
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

After commit/push, use browser automation, the user should already be logged in (if not ask the user to log in).

Find 5 high-quality targets. Optimize for conversation quality, not the easiest visible Connect buttons.

Target mix:

- 2 technical targets: engineers, managers, interns, or adjacent team members.
- 2 hiring targets: recruiters, hiring managers, talent partners, university recruiters, or clear people-team contacts.
- 1 free highest-ROI target with no extra restriction.

If the ideal mix is unavailable, choose the strongest substitutes and explain the gap.

Use a light scorecard while choosing: relevance to the role/team, natural conversation hook, sendability, and risk of wasting the request. Inspect profiles before choosing when possible. Prefer people whose background gives a natural reason to ask for insight. The message we send should be specific but also naturally invite a reply. Avoid targets whose profiles suggest they won't reply or will reject the request.

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
- Aim for 270-300 characters when possible; shorter is only acceptable when a longer version becomes awkward.
- Use straight apostrophes, not curly quotes.
- Be warm, specific, formal, direct, and normal.
- Make the note naturally invite a reply or short conversation.
- Do not make it a generic connect request.
- Do not claim a relationship, referral, or shared background that is not verified.
- Prefer `CS student` over naming UTD unless the school is a genuine conversation hook. The shorter intro leaves more room for a better personalized ask.
- Prefer direct asks like `I want to talk with you about...`, `I'd like to talk to you about...`, or `I'd appreciate your insights on...`, etc.
- Avoid vague `advice` language, generic admiration, inflated praise, pitch-like lines, and `10 minutes`.
- Use full sentences, and treat this as a business communication. Do not make it sound like a text to a friend. Full complete sentences.

More Guidance on Linkedin Message:

- Sound natural, warm, friendly, but professional. Avoid being too formal or too casual.
- Be specific about why you're reaching out to this person. Mention something from their profile or experience that caught your eye and relates to your ask.
- Ask for a quick conversation because you have some questions about their experience at the company, in the industry, or just in general.
- Make it interesting enough that they want to reply, but not so long that it becomes a chore to read. 270-300 characters is a good target when possible.
- Use high quality and engaging language, but avoid anything that sounds like a sales pitch or over-the-top praise. You want to come across as genuine and thoughtful, not like you're trying to impress them with fancy words.

### 13. Gate B Response

Stop after drafting outreach. Ask for feedback or explicit send approval.

If the user gives feedback, revise only targets/messages unless they ask for broader changes.

Do not send connection requests unless the user says exactly:

```text
LGTM SEND
```

### 14. Send Approved Connection Requests

Only after exact `LGTM SEND`, use the [$browser:control-in-app-browser](C:\\Users\\kushagra\\.codex\\plugins\\cache\\openai-bundled\\browser\\26.527.31326\\skills\\control-in-app-browser\\SKILL.md) skill and send requests one person at a time.

For each approved target:

1. Use only the approved note. Do not change it except replacing unsupported punctuation with typeable equivalents.
2. Open the approved LinkedIn profile URL.
3. Inspect the profile page and click the visible `Connect` button.
4. When the connection modal opens, verify it offers `Send without a note` and `Add a note`. Always click the actual visible `Add a note` button. Never click any path that says or implies sending without a note, and do not reuse coordinates from another profile.
5. Focus the visible `textarea[name="message"]` and type the normalized note character-by-character with keyboard events. Do not use fill, paste, clipboard writes, or bulk text entry.
6. Before sending, verify all of the following: the note box is visibly open, the full note is visibly present, the counter shows the expected `N/300`, the textarea value exactly matches the approved note, and the Send button is enabled.
7. Take one final look at the modal. If any check is ambiguous, stop and report instead of sending.
8. Click only the verified `Send invitation` button.
9. Report sent, skipped, and blocked requests.

Work through it all and get all 5 connections with notes sent.

### 15. Final Tracker Update

As the final workflow step, update `Application_Tracker.xlsx` once. Upsert the current application row using only the existing columns: `Company`, `Role`, `Role Type`, `Season`, `Location`, `Posting Link`, `Status`, `Date Applied`, `LinkedIn Reachouts`, `Notes`.

Use only these role type values: `Internship`, `New Grad`, `Other`.

Use only these status values: `need to apply`, `applied`, `OA`, `interview`, `rejected`, `offer`.

Kushagra treats a completed tailored packet as submitted. So when the tailoring workflow is done, set `Status` to `applied` and set `Date Applied` to the actual submission date if known; otherwise use the date the tailored packet was finalized. Add only actually messaged LinkedIn profile URLs to `LinkedIn Reachouts`, keep `Notes` short, and mention if the tracker could not be updated.

## Completion

The workflow is complete when the user-approved application packet is built, verified, committed/pushed, outreach targets/messages have passed Gate B, approved sends have either completed or been reported as blocked/skipped, and `Application_Tracker.xlsx` has been updated or the blocker is clearly reported.
