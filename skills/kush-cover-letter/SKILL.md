---
name: kush-cover-letter
description: Write or revise Kushagra's company-specific cover-letter.md for a target job using the current resume, llms.txt, company research, and job posting. Use when tailoring the company folder cover-letter.md under 03_Tailored_Applications with specific company fit, truthful experience mapping, professional tone, and one-page PDF readiness.
---

# Kush Cover Letter

## Purpose

Create a polished, specific cover letter for one company and role, using the user's real background and the company/job context.

Use this skill inside `$kush-app-workflow` or on its own when the user asks for a cover letter. If `$humanizer` is active, use it as a final polish pass after the factual draft is complete.

## Required Context

Read:

- The job posting or job URL.
- `llms.txt` for current profile context.
- Root cover letter template for baseline structure only.
- Company-specific `03_Tailored_Applications/<Company>/cover-letter.md` for edits.
- Company/product/team research from primary sources when available.

Hard rule: never edit the root cover letter template. Only edit the company-specific copy.

## Cover Letter Goals

The letter should:

- Sound like a specific person applying to a specific role, not a generic template.
- Connect 2-3 strongest experiences to the job requirements.
- Include one or two concrete company details from research.
- Be direct, warm, and confident without sounding inflated.
- Avoid filler, buzzword stacking, and generic admiration.
- Fit cleanly on one PDF page after build.

## Structure

Use this default shape unless the role calls for something different:

```text
Date / recipient block

Opening: role, company, and a specific reason this role makes sense.

Experience paragraph 1: strongest technical/project match.

Experience paragraph 2: second match, preferably with product/research/systems judgment.

Company fit paragraph: concrete company/team/product reason and how the user's background connects.

Close: concise interest and next step.

Signature/contact block
```

## Writing Rules

Do:

- Replace all placeholders.
- Remove tailoring checklists and template notes.
- Use exact role and company names.
- Match the job's strongest requirements to real projects, internships, research, and skills.
- Keep claims verifiable from `llms.txt`, resume, or user-provided context.
- Use active, human wording.
- Keep it one page.

Do not:

- Invent personal connections, referrals, company facts, metrics, or experience.
- Mention a company value/product unless it was researched or provided.
- Repeat the resume mechanically.
- Over-apologize for being a student or intern candidate.
- Use generic openings like "I am writing to apply" when a sharper opening is possible.
- Send or submit anything; only write the file and return control to the workflow.

## Humanizer Use

If `$humanizer` is explicitly active:

1. Draft the letter factually first.
2. Run the final prose through the humanizer skill conceptually.
3. Preserve all factual claims, dates, company names, and technical terms.
4. Keep the result concise enough for one page.

If `$humanizer` is not active, perform normal professional editing without claiming a humanizer pass.

## Output Expectations

When finished, summarize:

- Company-specific details used.
- Experiences emphasized.
- Any assumptions or facts the user should verify.
- Whether the cover letter still needs build/page verification.

If used inside `$kush-app-workflow`, return control to that workflow so it can build and verify PDFs.
