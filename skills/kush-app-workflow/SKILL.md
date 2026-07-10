---
name: kush-app-workflow
description: "Run Kushagra's end-to-end internship and new-grad application workflow: create company packets, coordinate resume and cover-letter tailoring, answer application prompts, build and verify one-page PDFs, enforce review gates, research and draft outreach, send LinkedIn requests only after exact LGTM SEND approval, update the tracker, clean artifacts, and commit and push once at the end. Use for full company application runs under 03_Tailored_Applications."
---

# Kush App Workflow

## Purpose

Coordinate the complete application run. Delegate writing judgment to the specialized skills and keep sequencing, approvals, deterministic scripts, tracker updates, cleanup, and Git operations consistent.

Use this invocation when practical:

```text
Use $humanizer, $kush-resume-tailor, $kush-cover-letter, $cold-message-writer, and $kush-app-workflow.

Company:
<company>

Job posting:
<posting text or exact URL>

Application questions:
<optional prompts and limits>

Extra notes:
<optional positioning, referral context, or constraints>
```

## Skill Boundaries

- At Gate A, load and use `$kush-resume-tailor`, `$kush-cover-letter`, and `$humanizer`. Apply humanizer to the cover letter and application answers, not resume bullets.
- At Gate B and for any LinkedIn, X/Twitter, DM, founder, recruiter, or referral message, load and use `$cold-message-writer` and `$humanizer`.
- Let `$kush-resume-tailor` own resume judgment and `$kush-cover-letter` own cover-letter judgment. Do not duplicate their detailed rules here.
- This workflow overrides generic no-note advice: LinkedIn requests use an approved note and require exact `LGTM SEND` before sending.

## Workspace Contract

Expected inputs and tools:

```text
llms.txt
resume.tex
Kushagra Bharti Cover Letter - General Template.md
Application_Tracker.xlsx
03_Tailored_Applications/
scripts/new-tailored-application.cmd
scripts/build-application.cmd
scripts/verify-application.cmd
scripts/update-tracker.cmd
scripts/cleanup-application.cmd
```

Hard rules:

- Never edit root `resume.tex` or the root cover-letter template during an application run.
- Edit only the target company's packet plus the tracker at the final data step.
- Put application answers in `03_Tailored_Applications/<Company>/application-questions.md`.
- Preserve the tracker schema unless the user requests a change.
- Never invent requirements, facts, metrics, dates, links, technologies, relationships, or experience.
- Preserve unrelated working-tree changes. If a protected root file changes unexpectedly, stop before committing.
- Use PowerShell-safe commands and existing scripts; do not use Bash heredocs in this Windows workspace.

Positioning rules for every outward-facing artifact:

- Lead with supported strengths, evidence, and fit. Never volunteer weaknesses, gaps, missing qualifications, or reasons to reject Kushagra.
- Omit unsupported technologies instead of writing caveats such as `while I do not have experience with X` or `although I have not used X`.
- Never apologize for being a student, call Kushagra inexperienced or `still early`, or frame ambition as compensation for a deficiency.
- Keep unsupported requirements in the private Gate A coverage summary, not in the resume, cover letter, application answers, or outreach.
- Do not let the resume, letter, answers, and outreach repeat the same evidence. Give each artifact a distinct job.

`llms.txt` is a symlink whose reported length may be zero. Read it directly with:

```powershell
Get-Content -LiteralPath .\llms.txt
```

## Approval Gates

Gate A covers the resume PDF, cover-letter PDF, and application answers. Stop for feedback or `lgtm`.

Gate B covers outreach targets and messages. Stop for feedback or exact send approval:

```text
LGTM SEND
```

Plain `lgtm` never authorizes outreach sends.

## Workflow

### 1. Intake and Packet Setup

1. Identify the company, role, role type, posting source, application questions, limits, and user notes.
2. Read `llms.txt`, root `resume.tex`, and the root cover-letter template.
3. Treat pasted posting and application-field text as authoritative. If only a URL is supplied, open that exact URL; do not reconstruct requirements or questions through web search.
4. Use `$chrome:control-chrome` when existing Chrome state matters. Use `$browser:control-in-app-browser` if Chrome is unavailable.
5. If the exact posting is inaccessible, report it and use only user-provided text unless the user requests broader recovery.
6. Treat related postings at one company as one packet unless the user requests separate packets.
7. Ask one concise question only when company, role, or another material input is genuinely ambiguous.

Create the packet:

```powershell
.\scripts\new-tailored-application.cmd "<Company>"
```

Refresh an existing packet from the current root sources when needed:

```powershell
.\scripts\new-tailored-application.cmd "<Company>" -Force
```

Before writing, set a compact internal plan:

```text
packet thesis -> artifact roles -> ATS map
```

Use enough primary-source company research to be specific. Keep artifacts distinct: resume = dense proof; cover letter = story and fit; application answers = exact prompt response; outreach = personal hook and reason to talk.

### 2. Prepare Gate A

#### Resume

Use `$kush-resume-tailor` and edit only the company-specific `resume.tex`. Build a compact `requirement -> evidence -> minimal edit` map. Preserve truth, the current resume structure, and a full one-page result.

#### Cover letter

Use `$kush-cover-letter`, then `$humanizer`, on the company-specific `cover-letter.md`. Keep its approved structure, make it additive to the resume, and produce one full clean page.

#### Application questions

When prompts exist:

1. Reproduce each exact prompt and limit.
2. Write a paste-ready, truthful answer in Kushagra's voice.
3. Answer directly; do not repeat the cover letter.
4. Lead with what the project or product is and why it matters, then use technical detail as proof.
5. Make the answer readable to a founder or hiring manager; do not lead with a list of tools, schemas, telemetry, or other internals.
6. Use concrete systems, decisions, results, and hard parts instead of abstract claims or fake-polished product language.
7. Never answer by foregrounding missing experience or unmet requirements. Use the strongest truthful adjacent evidence and omit the caveat.
8. Keep structure flexible and use only enough organization to make the answer clear and easy to paste.
9. When a maximum is provided, aim within 5% below it without filler and report the count.
10. Run `$humanizer` without changing facts, technical meaning, or the core answer.

#### Build and verify

```powershell
.\scripts\build-application.cmd "<Company>" -Clean
.\scripts\verify-application.cmd "<Company>"
```

Confirm both PDFs exist, render cleanly, contain no placeholders, and are one page. Inspect rendered previews for clipping, broken layout, incorrect company/role, or signature problems. Confirm every application prompt has a complete answer.

If a build fails, fix only the company-specific source and rebuild. If visual inspection is unavailable, say so and still complete file, page-count, text, placeholder, and link checks.

#### Gate A response

Return:

- Clickable PDF paths.
- Brief resume changes and cover-letter positioning.
- Compact ATS coverage: covered, partial, unsupported.
- Application answers and counts, if any.
- Assumptions and verification status.

Do not research LinkedIn targets, commit, or push before Gate A approval. After feedback, revise and repeat Gate A checks. After `lgtm`, continue without committing.

### 3. Research Outreach Targets

Use browser automation after Gate A approval. Prefer `$chrome:control-chrome` for the user's logged-in LinkedIn session and `$browser:control-in-app-browser` as fallback.

Find up to five strong, sendable targets. Adapt the mix:

- Tiny startup: founders, founding engineers, and technical leaders close to the product.
- Mid-sized company: relevant engineers or managers plus selective recruiting contacts.
- Large company: team-adjacent engineers or managers plus role-specific or university recruiters.
- Research organization: researchers, research engineers, team leads, and relevant recruiting contacts.
- Quant or finance company: desk or team engineers, researchers, technical managers, and specialized recruiters.

Optimize for role relevance, a genuine conversation hook, likely reply quality, and request value. Do not fill a quota with weak targets. Inspect profiles before selecting when possible.

### 4. Draft Gate B Outreach

Use `$cold-message-writer` and `$humanizer`. For each target, return name, role, profile URL, target type, selection reason, approved-note draft, and character count.

Message rules:

- Stay under 300 characters; aim for 270-300 only when every sentence earns its space.
- Use straight apostrophes and complete sentences.
- Be warm, specific, direct, professional, and natural.
- Mention the real profile, team, or company detail that makes this person worth contacting.
- Invite a quick conversation with an easy, concrete ask.
- For hiring targets, ask about relevant roles, teams, or where Kushagra's background may fit.
- For technical targets, ask about the specific work or technical area that created the connection.
- Prefer `CS student` over naming UTD unless the school creates a genuine hook.
- Vary the notes. Ground hunger, motivation, and willingness to learn in real context.
- Never mention missing qualifications or ask the recipient to overlook a gap.
- Avoid generic admiration, vague advice or perspective requests, inflated praise, sales language, unverified common ground, generic `I applied and would love to connect` phrasing, and default `10 minutes` asks.

Stop at Gate B. Revise drafts if requested. Do not send without exact `LGTM SEND`.

### 5. Send Approved LinkedIn Requests

Send one target at a time using only the approved note.

1. Prefer the direct invite URL when the vanity name is clear; otherwise use the verified target profile's Connect control.
2. Inspect the modal and stop on blocks, rate limits, challenges, rejection, or unexpected prompts.
3. Click the visible `Add a note`; never choose a no-note path.
4. Prefer `textarea[name="message"]`. Ignore hidden fields such as `g-recaptcha-response`.
5. Type character by character; do not fill, paste, use the clipboard, or bulk-insert text.
6. Verify the visible note, exact textarea value, expected counter, target identity, and enabled send button.
7. Prefer `button[aria-label="Send invitation"]` for the final action.
8. If any check is ambiguous, stop. Otherwise click only the verified send button.
9. Report sent, skipped, and blocked targets.

### 6. Tracker, Cleanup, and Git

Update the tracker once after outreach:

```powershell
.\scripts\update-tracker.cmd --company "<Company>" --role "<Role>" --role-type "<Internship|New Grad|Other>" --season "<Season>" --location "<Location>" --posting-link "<url>" --status "applied" --date-applied "<Month D, YYYY>" --linkedin-reachouts "<profile>" --notes "Tailored packet completed; LinkedIn outreach sent."
```

Use only existing columns, role types (`Internship`, `New Grad`, `Other`), and statuses (`need to apply`, `applied`, `OA`, `interview`, `rejected`, `offer`). Treat a finalized packet as applied. Use the known submission date or the packet-finalization date. Record only profiles actually messaged and report whether the helper inserted or updated the row.

Then:

1. Run `.\scripts\cleanup-application.cmd "<Company>"`.
2. Check Git status and confirm the protected root files are unchanged.
3. Stage the target packet and `Application_Tracker.xlsx`; ignore unrelated work unless the user said to include everything.
4. Commit once as `Tailor application for <Company>`.
5. Push the current branch.

The workflow is complete when approved artifacts are verified, Gate B is resolved, approved sends are completed or clearly reported, the tracker is updated, temporary artifacts are removed, and the final commit and push succeed.
