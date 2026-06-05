---
name: kush-resume-tailor
description: Conservatively tailor Kushagra's company-specific LaTeX resume for a job posting while preserving truth, full one-page density, links, protected LaTeX/header/education/skills rules, fixed experience chronology, rare project swaps from llms.txt, and role-specific ATS keyword emphasis. Use when optimizing company-folder resume.tex files under 03_Tailored_Applications inside the internship/new-grad application workflow.
---

# Kush Resume Tailor

## Purpose

Tailor one company-specific `resume.tex` for one target job. Kushagra's base resume is already strong and full; the goal is to make the existing page read slightly closer to the posting, not rebuild it.

Default to minimal, targeted edits: keyword emphasis, clearer role alignment, and small bullet rewrites. Do not shorten the resume into a sparse page, restructure the resume, or aggressively swap content just because a posting has a few different keywords.

Use this skill inside `$kush-app-workflow` or on its own. If `$humanizer` is active, do not use it on the resume unless the user explicitly asks; resume bullets should stay precise, dense, and technical.

## Required Context

Read:

- Job posting text or URL. Get content directly from the URL when possible, and use web search effectively.
- `llms.txt` for current profile context, project inventory, metrics, links, and supported skills.
- Root `resume.tex` for baseline context only.
- Company-specific `03_Tailored_Applications/<Company>/resume.tex` for edits.

Hard rules:

- Never edit root `resume.tex`. Only edit the company-specific copy.
- Never invent experience, metrics, titles, dates, tools, awards, publications, GPA, certifications, company names, or links.
- Never add claims that appear only in the job description.
- Keep the resume truthful, dense, and one page.
- Strong framing is acceptable when it is clearly grounded in `llms.txt` or the base resume.

## Pre-Edit Strategy

Before editing, build an internal strategy:

```text
role type -> top requirements -> supported evidence -> minimal resume edit
```

Use the strategy to decide what to touch. If the current resume already covers the role well, make only the smallest useful changes.

Prioritize:

1. Required languages, frameworks, tools, and domains.
2. Core role nouns and responsibilities.
3. Evidence of impact: metrics, shipped systems, research results, tests, users, production context.
4. Soft requirements only when concrete work demonstrates them.

Unsupported requirements belong in the final summary, not in the resume.

## Common Rules

- Let the job description drive small edits, but assume most postings need only minimal keyword and emphasis changes.
- Build a compact ATS map before editing: `requirement -> supported evidence -> minimal edit`.
- Preserve the resume's density. Do not delete bullets or shrink the resume into a visibly underfilled page just because some bullets are less directly relevant.
- Do not edit the LaTeX preamble, custom commands, header/contact block, spacing commands, section structure, or link syntax.
- Add exact job keywords when supported by `llms.txt`, root resume, company-specific resume, or user notes.
- Keep the resume one full page. If it spills or becomes underfilled, fix content first and preserve density before touching formatting.

## Education Rules

- The only normal edit above Experience is the Education date.
- For internship roles, set Education to `August 2023 -- December 2027`.
- For new-grad roles, set Education to `August 2023 -- May 2027`.
- Do not leave the base `May/Dec 2027` placeholder.
- Do not change the school, degree, minor, certificate, coursework, or location unless the user explicitly asks.

## Technical Skills Rules

- Treat Technical Skills as no-touch by default.
- Only add one or two supported skills if the posting explicitly values them, the evidence exists in `llms.txt` or the resume, and the addition does not require removing or reordering existing skills.
- Never remove, reorder, rename, or broadly tune skills for ATS.

## Experience Rules

- Do not swap, remove, or reorder experiences. Experience chronology is fixed.
- Preserve each experience's title, employer, date range, location, and link.
- Do not change the number of experience bullets by default. Rewrite bullets in place.
- Rewrites should preserve high quality and density, foreground supported keywords, and maintain ATS coverage.
- Rewrites should be clear and maintain the same voice and writing style as the rest of the resume.
- Do not broadly compress experience content. If a bullet is lower-fit, usually rewrite it to foreground supported job language instead of cutting it.

## Project Rules

- Preserve project links when known; never invent missing links.
- Keep project-heading right-side date/status arguments empty.
- Preserve project order and bullet count unless a rare project swap truly requires adjustment.
- Swap a project only in rare, high-signal cases where the posting has a clear domain match that the current projects miss, such as RL, LLM agents, computer vision, web-dev, quant, backend systems, etc.
- Treat swaps as rare judgment calls. The existing three projects are already strong contenders.
- When a rare swap is justified, either swap a project cleanly or reduce existing project content to make space for the new one. Either way, preserve a full, dense page.
- Replace or shorten the lowest-fit current project. Do not append extra projects.
- Choose based on the role, the posting, and the strongest evidence in `llms.txt`, not a fixed project matrix.
- Use project links only when present in `llms.txt`, root resume, company-specific resume, or user input. If the best project lacks a known link, include it without inventing a URL and flag the missing link in the summary.

## Bullet Standard

Prefer this shape:

```text
Action + technical work + concrete outcome/scale + tool/system/context
```

Good bullets are specific enough to be credible and compact enough to fit.

Use the Google X-Y-Z formula as a light quality check, especially when rewriting an existing bullet:

```text
Accomplished [X] as measured by [Y], by doing [Z]
```

Interpret it as:

- `X`: the outcome, improvement, shipped system, or research/product impact.
- `Y`: the metric, scale, benchmark, user count, latency/cost/result, or other proof.
- `Z`: the technical method, tool, architecture, or implementation choice that drove the result.

Most existing bullets already roughly follow this pattern. Do not rewrite strong bullets just to make the formula explicit. Use it to preserve outcome, proof, and method when making small role-specific edits. Again, rewriting should be the exception, not the default. The resume should still read like the original, just slightly tailored. You can just insert and reword keywords in place without fully restructuring a bullet.

When rewriting:

- Preserve strong metrics and concrete outcomes. Do not weaken a bullet by adding nonsensical metrics; the metrics already there are there for a reason.
- Prefer technical nouns over filler adjectives.
- Avoid generic ATS stuffing.
- Avoid weakening a strong bullet just to insert one keyword.
- Avoid duplicate evidence across bullets unless the role genuinely rewards repeated emphasis.

## Final Checks

Before returning control:

- The resume should still look like a full, dense, high-signal one-page resume.
- The edit should read as targeted tailoring, not a reconstructed resume.
- Unsupported requirements should be reported, not forced into the resume.
- Root `resume.tex` should remain untouched.
- Technical Skills should be unchanged unless the narrow addition rule was intentionally used.
- Experience chronology, titles, employers, dates, locations, and links should be unchanged.
- Project headings should still have empty right-side date/status arguments.

## Verification

After edits, the workflow should:

- Build the PDF.
- Confirm page count is one.
- Render or visually inspect the PDF.
- Check for clipping, broken links, missing sections, bad line wraps, wrong company, wrong role, and placeholder text.
- Check that the page is filled appropriately; if the resume wastes major vertical space, restore high-signal supported content.
- Extract text when practical and confirm the most important job keywords are present.

If using this skill standalone, run the same verification when practical. If build or visual verification cannot run, state exactly what remains unverified.

## Output

Summarize:

- Key resume changes.
- Keywords added or emphasized.
- Compact ATS coverage: covered, partially covered, and unsupported requirements.
- Any Technical Skills additions and their evidence.
- Project swaps, shortened projects, and link assumptions.
- Important unsupported requirements.
- Build/page verification status.

If used inside `$kush-app-workflow`, return control so it can build, verify, and present Gate A.
