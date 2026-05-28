---
name: kush-resume-tailor
description: Tailor Kushagra's company-specific LaTeX resume for a job posting while preserving truth, one-page layout, links, and the root resume template. Use when optimizing company-folder resume.tex files under 03_Tailored_Applications for ATS alignment, project swaps from llms.txt, role-specific keyword coverage, and concise resume edits inside the application workflow.
---

# Kush Resume Tailor

## Purpose

Optimize the company-specific `resume.tex` for one target job. Treat the resume as already full: every improvement should come from rewriting, reordering, compressing, or swapping content, not from adding net length. Use judgment from the job description and `llms.txt`; do not force predefined role buckets or fixed project matrices.

Use this skill inside `$kush-app-workflow` or on its own. If `$humanizer` is active, do not use it on the resume unless the user explicitly asks; resume bullets should stay precise and compact.

## Required Context

Read:

- Job posting text or URL.
- `llms.txt` for current profile context, project inventory, metrics, links, and voice.
- Root `resume.tex` for baseline context only.
- Company-specific `03_Tailored_Applications/<Company>/resume.tex` for edits.

Hard rule: never edit root `resume.tex`. Only edit the company-specific copy.

Eligibility rule: for internship roles, set the education date to `August 2023 -- December 2027` by default. Use `August 2023 -- May 2027` only for new-grad roles or when the user explicitly asks. Do not change the root resume while tailoring a company-specific packet unless the user is updating the base resume itself.

## ATS Map

Before editing, build an internal map:

```text
Requirement / keyword -> Evidence from resume or llms.txt -> Resume edit
```

Use this map to decide what to change. Do not add unsupported terms. If a requirement is important but unsupported, note it in the final summary instead of inventing evidence.

Prioritize:

1. Required languages, frameworks, tools, and domains.
2. Core responsibilities and role nouns.
3. Evidence of impact: metrics, shipped systems, research results, tests, users, production context.
4. Soft requirements only when they can be shown through concrete work.

## Edit Strategy

Use this order:

1. Rewrite and reorder bullets within experiences and projects so the strongest fit is seen first.
2. Rewrite existing bullets to use truthful job language.
3. Compress low-fit bullets to make room.
4. Swap projects if another project from `llms.txt` fits materially better.
5. Remove lower-fit content before touching formatting.

Do not use font, margin, or spacing changes as the first fix for overflow. Content quality comes first.

Technical Skills is a hard no-touch section unless the user explicitly asks. Do not reorder, add, remove, or tune skills for ATS. Tailoring should happen through Experience bullets and Projects.

Experience chronology is a hard rule: keep the Experience section in reverse chronological order. Do not reorder jobs by relevance. Tailor the skills, bullets, projects, and emphasis instead.

The resume should be one full, high-signal page. One page is not enough if the bottom third is empty. If the resume looks underfilled after trimming, add back the strongest supported content until the page is used well without spilling.

## Project Swaps

When the current projects do not fit the role, choose replacements from `llms.txt`.

Rank projects by:

- Direct match to the role's domain and technologies.
- Concrete metrics or outcomes.
- Technical depth.
- Recency and credibility.
- Availability of a real link.

Rules:

- Replace a lower-fit project; do not append a third project unless a shorter layout already supports it.
- Preserve existing LaTeX link patterns when adding a linked project.
- Use project links only when present in `llms.txt`, root resume, or user input.
- If the best project lacks a known link, include the project without inventing a URL and flag the missing link in the summary.
- Trust the strongest evidence for the specific posting; do not mechanically pick projects from a preset category list.

## Bullet Standard

Prefer this shape:

```text
Action + technical work + concrete outcome/scale + tool/system/context
```

Good bullets are specific enough to be credible and compact enough to fit. Do not force a metric where none exists; a real technical detail is better than a fake number.

## Editing Rules

Do:

- Use exact job keywords when supported by Kushagra's actual work.
- Prefer technical nouns over filler adjectives.
- Keep metrics, titles, dates, employers, schools, and awards exact.
- Preserve LaTeX syntax, links, commands, spacing conventions, and section structure.
- Leave the Technical Skills section unchanged unless the user explicitly asks to edit it.
- Keep project-heading right-side date/status arguments empty; project dates do not add value here.
- Keep the resume to one page.
- Use the full page well; avoid visibly underfilled resumes.

Do not:

- Invent experience, metrics, titles, dates, tools, awards, publications, GPA, certifications, or company names.
- Edit the root resume template.
- Add generic ATS stuffing.
- Add claims that appear only in the job description.
- Expand the resume beyond one page. If it spills, compress, swap, or remove lower-fit content.

## Verification

After edits, the workflow should:

- Build the PDF.
- Confirm page count is one.
- Render or visually inspect the PDF.
- Check for clipping, broken links, missing sections, and bad line wraps.
- Check that the page is filled appropriately; if the resume wastes major vertical space, restore high-signal supported content.
- Extract text when practical and confirm the most important job keywords are present.

If using this skill standalone, state whether build/page verification still needs to run.

## Output

Summarize:

- Key resume changes.
- Keywords added or emphasized.
- Compact ATS coverage: covered, partially covered, and unsupported requirements.
- Project swaps and link assumptions.
- Important unsupported requirements.
- Build/page verification status.

If used inside `$kush-app-workflow`, return control so it can build, verify, and present Gate A.
