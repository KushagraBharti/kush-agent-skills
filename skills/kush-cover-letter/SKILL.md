---
name: kush-cover-letter
description: Write or revise Kushagra's company-specific cover-letter.md for a target job using the current resume, llms.txt, company research, and job posting. Use when tailoring company-folder cover-letter.md files under 03_Tailored_Applications for specific company fit, concise human voice, truthful experience mapping, application-question consistency, signature links, and one-page PDF readiness.
---

# Kush Cover Letter

## Purpose

Create a specific, concise cover letter for one company and role. The letter should sound like Kushagra: direct, ambitious, technical, and human without sounding dramatic or generic. Use the job description, company research, current resume, and `llms.txt` with judgment while preserving the root template's basic shape unless the user asks otherwise.

Use this skill inside `$kush-app-workflow` or on its own. If `$humanizer` is active, use it only after the factual draft is complete.

## Required Context

Read:

- Job posting text or URL.
- `llms.txt` for current profile context, projects, motivation, links, and voice.
- Root cover letter template for baseline structure only.
- Company-specific `03_Tailored_Applications/<Company>/cover-letter.md` for edits.
- Company/product/team research from primary sources when available.
- Application questions, if provided, so the letter does not duplicate those answers.

Hard rule: never edit the root cover letter template. Only edit the company-specific copy.

Template rule: the company-specific `cover-letter.md` starts from `Kushagra Bharti Cover Letter - General Template.md`. Preserve that template's overall structure: direct greeting, short opening, evidence bullets or compact evidence section, company-fit paragraph, ownership/personality paragraph, close, and linked signature. Tailor the placeholders; do not replace the format with a generic cover-letter essay unless the user explicitly asks.

## Goals

The letter should:

- Connect 2-3 strongest experiences to the role.
- Use one or two concrete company/team/product details.
- Show motivation and work ethic through evidence, not self-labels.
- Let personality through with direct, specific lines about ownership, learning speed, taste, or high standards when supported by context.
- Stay brief while still having substance.
- Fit cleanly on one PDF page.

Target length: usually 350-450 words, shorter if the rendered PDF needs it.

## Voice

Use:

- Straightforward, specific sentences.
- Short paragraphs with one clear point each.
- Concrete examples over abstract claims.
- Confident but normal language.

Avoid:

- Corny lines, grand mission language, and over-polished corporate tone.
- Generic admiration such as "I am inspired by your commitment to innovation."
- Repeating company values as slogans.
- Saying "hardworking" directly when an example can prove it.

When using company values, translate them into behavior:

```text
Value -> what the team seems to reward -> concrete evidence from Kushagra's work
```

## Structure

Default shape should follow the root template:

```text
Date / recipient block

Greeting.

Opening: role, company, and the clearest specific reason this role fits.

Evidence section: 2-3 bullets or compact paragraphs mapping real work to the role.

Company fit: plain, specific reason this company/team is interesting.

Personality/ownership: concise line about how Kushagra works, grounded in evidence.

Close: concise interest.

Signature block
```

Do not force all sections if a shorter letter is stronger, but keep the root template's recognizable shape when possible.

## Signature

Use this default signature unless the user asks otherwise:

```markdown
Sincerely,
Kushagra Bharti
Student | B.S. Computer Science
The University of Texas at Dallas
[LinkedIn](https://www.linkedin.com/in/kushagra-bharti/) | [Personal Site](https://www.kushagrabharti.com) | [GitHub](https://github.com/KushagraBharti/)
```

In the actual `cover-letter.md`, keep signature lines consecutive with no blank lines inside the signature block. Use Markdown links instead of raw long URLs when supported by the renderer. Verify the rendered PDF does not wrap the signature awkwardly.

## Writing Rules

Do:

- Replace all placeholders and template notes.
- Use exact role and company names.
- Match the job's strongest requirements to real work.
- Keep claims verifiable from `llms.txt`, resume, or user-provided context.
- Keep the letter distinct from application-question answers.
- Before finalizing, make sure each paragraph answers either "why this role/company" or "why this evidence matters."
- Tighten aggressively if it spills to a second page.

Do not:

- Invent company facts, referrals, personal connections, experience, metrics, or links.
- Mention values/products unless researched or user-provided.
- Apologize for being a student or intern candidate.
- Use generic openings like "I am writing to apply" when a sharper opening is possible.
- Submit or send anything.

## Humanizer Pass

If `$humanizer` is active:

1. Draft factually first.
2. Remove inflated wording, filler transitions, cliches, and AI-ish rhythm.
3. Preserve company names, dates, metrics, technical terms, and links.
4. Rewrite to sound natural, have no corny lines, be concise, professional but human, and maintain the same structure.
5. Keep the final letter one-page ready.

## Output

Summarize:

- Company details used.
- Experiences emphasized.
- Any assumptions the user should verify.
- Signature/link handling.
- Build/page verification status.

If used inside `$kush-app-workflow`, return control so it can build, verify, and present Gate A.
