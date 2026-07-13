---
name: kush-app-workflow
description: End-to-end internship and new-grad application workflow for Kushagra that creates company-specific packets, coordinates kush-resume-tailor, kush-cover-letter, humanizer, and cold-message-writer, answers prompts, builds/verifies one-page PDFs, gates approval, researches LinkedIn targets, drafts outreach, sends only after explicit LGTM SEND approval, updates the tracker, cleans artifacts, and commits/pushes once at the end.
---

# Kush App Workflow

## Purpose

Run the full internship and new-grad application loop from job posting to approved PDFs, written application answers, LinkedIn target research, outreach draft review, approved connection sends, tracker update, cleanup, and one final Git commit/push.

This skill is procedural. Do not assume personal facts outside the workspace, `llms.txt`, resume, cover letter template, job posting, company research, or explicit user notes.

## Preferred Invocation

```text
Use $humanizer, $kush-resume-tailor, $kush-cover-letter, $cold-message-writer, and $kush-app-workflow.

Company:
<company>

Job posting:
<paste job posting or URL>

Application questions:
<optional writing prompts, limits, or application text boxes>

Extra notes:
<optional positioning, tone, referral context, constraints>
```

Required skill use:

- Gate A must load and use `$kush-resume-tailor`, `$kush-cover-letter`, and `$humanizer`. `$humanizer` applies to cover letters and application-question answers, not resume bullets.
- Gate B, LinkedIn notes, Twitter/X messages, DMs, founder messages, and referral asks must load and use `$cold-message-writer` plus `$humanizer`.
- `$kush-app-workflow` owns sequencing and approval gates. `$kush-resume-tailor` owns resume judgment. `$kush-cover-letter` owns letter judgment. `$cold-message-writer` owns first-touch outreach judgment. `$humanizer` owns final prose cleanup without blurring facts or technical precision.
- For LinkedIn connection requests, this workflow overrides `$cold-message-writer`'s generic "no note" LinkedIn advice. Draft notes, get approval, and send only after exact `LGTM SEND`.

## Workspace Contract

Expected files:

```text
llms.txt
resume.tex
Kushagra Bharti Cover Letter - General Template.md
scripts/new-tailored-application.cmd
scripts/build-application.cmd
scripts/verify-application.cmd
scripts/cleanup-application.cmd
scripts/update-tracker.cmd
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
6. If the user provides application questions or application-field text, treat that as authoritative. Do not rediscover or replace it from the posting.

On Windows, `llms.txt` may be a symlink whose file length appears as `0`. Do not check size/length to decide whether it has content. Read it directly with:

```powershell
Get-Content -LiteralPath .\llms.txt
```

Use PowerShell-safe commands and existing scripts. Do not use Bash heredocs in this Windows workspace.

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

Keep posting intake simple and source-faithful:

- If the user pasted the posting or application questions, use that text directly.
- If the user supplied only a job link, open that exact link and read it. Prefer `$chrome:control-chrome`; `$browser:control-in-app-browser` is also acceptable.
- Do not use web search to reconstruct job requirements or application questions.
- If the exact linked posting is inaccessible, blocked, expired, or requires login, report that clearly and use only user-provided posting text unless the user asks for broader recovery.

For company/product context, use only enough primary-source research to make the packet specific. Avoid padding the resume or cover letter with vague praise.

Before editing, set a compact internal plan:

```text
packet thesis -> artifact roles -> compact ATS map
```

Keep the artifacts non-duplicative: resume = dense proof, cover letter = additive story/fit, application answers = exact prompt response, LinkedIn notes = small hook plus reason to talk.

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
- For internship roles, keep Education at `Expected Dec 2027`.
- For new-grad roles, change only the Education date to `Expected May 2027`.
- Do not add an enrollment-start date.
- Do not change the school, degree, minor, certificate, coursework, or location unless the user explicitly asks.

Technical Skills rules:

- Use Technical Skills to maximize supported ATS keyword coverage.
- If a technology, platform, framework, method, or domain term appears in the posting and is supported by `llms.txt`, the root resume, the company-specific resume, or user notes, add the exact keyword to the appropriate existing category.
- Add all materially relevant supported matches, not just one or two.
- Never invent unsupported skills.
- Preserve existing category names, category order, and overall structure. Do not regroup, rename, or reorder categories.
- Do not remove existing skills unless the user explicitly asks.

Experience and Research rules:

- Do not swap, remove, or reorder Experience or Research entries. Keep each entry in its current section; do not move entries between Experience and Research.
- Preserve each entry's title, organization, date range, location, and link.
- Do not change the number of bullets for an entry by default. Rewrite bullets in place. Rewrites should preserve high quality and density, foreground supported keywords, and maintain ATS coverage. Rewrites should be clear and maintain the same voice and writing style as the rest of the resume.
- Do not broadly compress Experience or Research content. If a bullet is lower-fit, usually rewrite it to foreground supported job language instead of cutting it.

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
- Connect 2-3 strongest items across Experience, Research, and Projects to the role.
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
6. Lead with what the project/product is and why it matters, then add the technical proof. Do not answer as a list of internals.
7. Keep the reader experience strong: clear, project-first, readable to a founder or hiring manager, with enough technical detail to prove depth.
8. Respect word/character limits. When a max is provided, aim to land within 5% of it without adding filler. Include a quick count or note for the user.
9. Keep structure flexible; use only enough organization to make the answer clear and easy to paste.

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

Run:

```powershell
.\scripts\verify-application.cmd "<Company>"
```

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

### 10. After Gate A Approval

After Gate A `lgtm`:

1. Continue to LinkedIn target research.
2. Do not commit or push yet. Commit once at the end after tracker update and cleanup.

### 11. LinkedIn Target Research

After Gate A approval, use browser automation. Prefer `$chrome:control-chrome` for LinkedIn because it uses the user's logged-in Chrome session; `$browser:control-in-app-browser` is acceptable if Chrome is unavailable. If the user is not logged in, ask them to log in.

Find 5 high-quality targets. Optimize for conversation quality, not the easiest visible Connect buttons.

Target mix:

- Tiny startup: prioritize founders, founding engineers, and technical leaders.
- Mid-sized company: prioritize relevant engineers and managers plus selective hiring contacts.
- Large company: prioritize team-adjacent engineers and managers plus role-specific or university recruiters.
- Research organization: prioritize researchers, research engineers, team leads, and relevant recruiters.
- Quant or finance company: prioritize team engineers, researchers, technical managers, and specialized recruiters.

If the ideal mix is unavailable, choose the strongest substitutes and explain the gap.

Use a light scorecard while choosing: relevance to the role/team, natural conversation hook, sendability, and risk of wasting the request. Inspect profiles before choosing when possible. Prefer people whose background gives a natural reason to ask for insight. The message we send should be specific but also naturally invite a reply. Avoid targets whose profiles suggest they won't reply or will reject the request.

### 12. Draft Outreach

Use `$cold-message-writer` and `$humanizer`.

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
- Prefer direct quick-chat asks like `If you have a quick minute, I'd love to talk about...`, `I wanted to ask if we could hop on a quick call to talk about...`, or `I'd like to talk with you about...`.
- For recruiters, talent partners, university recruiters, and hiring targets, ask for a quick call to talk about roles at the company, the target team, or where Kushagra's background may fit. Do not ask vague questions like what makes an applicant stand out.
- For technical targets, name the specific thing from their work/profile and ask to talk about that topic directly, e.g. `If you have a quick minute, I'd love to talk about AV perception and mapping work.`
- Avoid vague `advice` language, generic `perspective` language, generic admiration, inflated praise, pitch-like lines, and `10 minutes`.
- Use full sentences, and treat this as a business communication. Do not make it sound like a text to a friend. Full complete sentences.

More Guidance on Linkedin Message:

- Sound natural, warm, friendly, but professional. Avoid being too formal or too casual.
- Be specific about why you're reaching out to this person. Mention something from their profile or experience that caught your eye and relates to your ask.
- Ask for a quick conversation directly. For hiring targets, make the ask about roles at the company or where Kushagra may fit. For technical targets, make the ask about the specific technical topic or team work that made the target worth contacting.
- Make it interesting enough that they want to reply, but not so long that it becomes a chore to read. 270-300 characters is a good target when possible.
- Use high quality and engaging language, but avoid anything that sounds like a sales pitch or over-the-top praise. You want to come across as genuine and thoughtful, not like you're trying to impress them with fancy words.
- Vary the notes. It is fine to signal hunger, motivation, willingness to learn fast, and real desire for the opportunity, but ground it in a specific reason the person/company is worth reaching out to.

### 13. Gate B Response

Stop after drafting outreach. Ask for feedback or explicit send approval.

If the user gives feedback, revise only targets/messages unless they ask for broader changes.

Do not send connection requests unless the user says exactly:

```text
LGTM SEND
```

### 14. Send Approved Connection Requests

Only after exact `LGTM SEND`, send requests one person at a time. Prefer `$chrome:control-chrome`; `$browser:control-in-app-browser` is also acceptable.

For each approved target:

1. Use only the approved note. Do not change it except replacing unsupported punctuation with typeable equivalents.
2. Prefer the direct invite URL when the profile vanity is clear: `https://www.linkedin.com/preload/custom-invite/?vanityName=<profile-vanity>`. Otherwise open the approved LinkedIn profile URL and click the visible, target-specific `Connect` or `Invite <name> to connect` control.
3. Inspect the modal before clicking. Stop if LinkedIn blocks, rate-limits, challenges, rejects the note, or shows an unexpected prompt.
4. Always click the actual visible `Add a note` button. Never click any path that says or implies sending without a note, and do not reuse coordinates from another profile.
5. If `Add a note` is visible but flaky, retry the same target safely using the direct invite surface or a verified forced click on `Add a note` only. Do not force-click `Send`.
6. Focus the visible note textarea, preferably `textarea[name="message"]`, and type the normalized note character-by-character with keyboard events. Ignore hidden fields such as `g-recaptcha-response`. Do not use fill, paste, clipboard writes, or bulk text entry.
7. Before sending, verify all of the following: the note box is visibly open, the full note is visibly present, the counter shows the expected `N/300`, the textarea value exactly matches the approved note, and the Send button is enabled. Prefer `button[aria-label="Send invitation"]` when selecting the final send button.
8. Take one final look at the modal. If any check is ambiguous, stop and report instead of sending.
9. Click only the verified `Send invitation` button.
10. Report sent, skipped, and blocked requests.

Try to complete all approved sends. Report any target LinkedIn blocks or makes ambiguous.

### 15. Final Tracker Update

As the final data step, update `Application_Tracker.xlsx` once. Prefer:

```powershell
.\scripts\update-tracker.cmd --company "<Company>" --role "<Role>" --role-type "Internship" --season "<Season>" --location "<Location>" --posting-link "<url1>" --posting-link "<url2>" --status "applied" --date-applied "<Month D, YYYY>" --linkedin-reachouts "<url1>" --linkedin-reachouts "<url2>" --notes "Tailored packet completed; LinkedIn outreach sent."
```

Upsert the current application row using only the existing columns: `Company`, `Role`, `Role Type`, `Season`, `Location`, `Posting Link`, `Status`, `Date Applied`, `LinkedIn Reachouts`, `Notes`.

Use only these role type values: `Internship`, `New Grad`, `Other`.

Use only these status values: `need to apply`, `applied`, `OA`, `interview`, `rejected`, `offer`.

Kushagra treats a completed tailored packet as submitted. So when the tailoring workflow is done, set `Status` to `applied` and set `Date Applied` to the actual submission date if known; otherwise use the date the tailored packet was finalized. Add only actually messaged LinkedIn profile URLs to `LinkedIn Reachouts`, keep `Notes` short, and report whether the helper printed `tracker_action=inserted` or `tracker_action=updated`.

### 16. Cleanup And Final Commit

After the tracker update:

1. Run `.\scripts\cleanup-application.cmd "<Company>"`.
2. Run `git status`.
3. Confirm root `resume.tex` and the root cover-letter template were not edited.
4. Stage the target company packet and `Application_Tracker.xlsx`. Ignore unrelated untracked files unless they block the task.
5. Commit once with:

```text
Tailor application for <Company>
```

6. Push the current branch.

## Completion

The workflow is complete when the user-approved application packet is built and verified, outreach targets/messages have passed Gate B, approved sends have either completed or been reported as blocked/skipped, `Application_Tracker.xlsx` has been updated or the blocker is clearly reported, temp artifacts are cleaned, and the final commit/push is done.
