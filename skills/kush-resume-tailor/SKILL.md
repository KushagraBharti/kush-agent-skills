---
name: kush-resume-tailor
description: Conservatively tailor Kushagra's company-specific LaTeX resume for internship or new-grad postings while preserving truth, the current Education/Experience/Projects/Research/Technical Skills structure, fixed experience and research entries, strong one-page density, links, supported ATS coverage, and selective project swaps. Use for resume.tex files under 03_Tailored_Applications.
---

# Kush Resume Tailor

## Purpose

Make the current resume read closer to one target role without rebuilding it. Preserve its strength, structure, truth, and full one-page density. Prefer small, high-signal edits; use larger changes only when the role clearly rewards them.

Do not run `$humanizer` on resume bullets unless the user explicitly asks. Resume language should remain compact, technical, and precise.

## Required Context

Read:

- User-provided posting text, or the exact posting URL when only a link is supplied.
- Root `resume.tex` as the current factual and structural baseline.
- `llms.txt` for expanded project inventory, supported technologies, metrics, and links.
- The company-specific `03_Tailored_Applications/<Company>/resume.tex` to edit.
- Explicit user notes.

Treat pasted posting text as authoritative. Do not reconstruct job requirements through web search. When factual sources conflict, use the current root resume as the latest authority.

## Hard Rules

- Never edit root `resume.tex`; edit only the company-specific copy.
- Never invent experience, responsibilities, metrics, titles, dates, tools, awards, publications, education, companies, or links.
- Never add a claim merely because the posting requests it.
- Preserve whether work is completed, active, newly started, or upcoming; do not change factual tense or imply unsupported accomplishments.
- Preserve the LaTeX preamble, commands, header, contact block, spacing system, section structure, and link syntax.
- Keep the resume truthful, interview-defensible, dense, and exactly one page.

The canonical section order is:

```text
Education -> Experience -> Projects -> Research -> Technical Skills
```

## Strategy

Before editing, build a compact map:

```text
role type -> top requirements -> supported evidence -> smallest strong edit
```

Prioritize required technologies and domains, core responsibilities, and concrete evidence of impact. Prefer strong evidence over shallow keyword matching. If the current resume already fits, change very little. Report unsupported requirements instead of forcing them into the page.

Position the resume around what Kushagra has done and can prove:

- Never add negative framing, qualification disclaimers, missing-skill callouts, or language that makes Kushagra sound less capable.
- Do not write `while I do not have X`, `although I have not used X`, or any equivalent caveat. Omit the unsupported requirement and report it only in the private ATS summary.
- Use strong supported framing when it improves clarity, but do not inflate scope, ownership, seniority, or results.
- Prioritize evidence in this order: required technical/domain fit, role responsibilities, measured impact or shipped work, then concrete soft-skill evidence.
- Keep edits consistent with the resume's existing voice. The result should look intentionally tailored, not reconstructed around the posting.

## Education

- Keep the default internship graduation date as `Expected Dec 2027`.
- For a new-grad role, change only the graduation date to `Expected May 2027`.
- Do not introduce enrollment-start dates.
- Preserve the school, degree, minor, certificate, coursework, and location unless the user explicitly requests a change.

## Experience and Research

Apply the same protections to both sections:

- Keep every entry in its current section. Never move Research into Experience or Experience into Research.
- Do not remove, swap, or reorder entries. Preserve chronology.
- Preserve organization, title, date range, location, and link.
- Preserve bullet counts by default and rewrite bullets in place.
- Preserve strong metrics, scope, and technical detail.
- Foreground supported role language without weakening the original evidence.
- Do not broadly compress lower-fit work; prefer a small emphasis change or leave it alone.
- Avoid duplicating the same proof across multiple bullets or sections.

## Projects

Projects are the primary flexible content surface.

- Preserve links; never invent a missing URL.
- Keep project-heading right-side date/status arguments empty.
- Preserve the current projects, order, and bullet counts by default.
- Swap or reorder projects when a materially stronger, supported role match justifies it.
- Compare role fit, project strength, interview depth, ATS value, and overall resume balance.
- Prefer one clean swap over appending another project or weakening several strong projects.
- Replace or shorten the lowest-value project for the target role; never remove Experience or Research to make project changes fit.
- Use project details and links only when supported by the root resume, `llms.txt`, company resume, or user notes.
- If a selected project has no verified link, include it without inventing one and flag the missing link in the summary.
- Use bold sparingly for genuinely high-signal technical or product claims.

## Technical Skills

Use Technical Skills as an active ATS index, not a frozen section.

- Extract exact technologies, platforms, methods, and domain terms from the posting.
- Add supported high-value keywords that improve ATS coverage.
- Regroup, reorder, or rename categories when it makes the supported match clearer.
- Prefer expanding coverage; remove or replace low-value terms only when needed for one-page fit or clarity.
- Keep categories readable and avoid repetitive keyword dumping.
- Require evidence in the root resume, `llms.txt`, company resume, or user notes. Never add unsupported tools.
- Preserve the section's current bottom placement.

## Bullet Standard

Prefer:

```text
action + technical work + outcome or scale + system context
```

Use Google X-Y-Z only as a quality check:

```text
Accomplished [X] as measured by [Y], by doing [Z]
```

Interpret `X` as the outcome, `Y` as real proof or scale, and `Z` as the technical method. Not every strong bullet needs all three explicitly.

When rewriting:

- Do not rewrite a strong bullet merely to force the formula or insert one keyword.
- Preserve real metrics and outcomes; never manufacture, stretch, or add nonsensical measurements.
- Prefer technical nouns and concrete actions over filler adjectives or vague ownership claims.
- Avoid generic ATS stuffing, repeated keywords, and duplicate evidence across bullets.
- Do not lead with a tool list when the system, hard problem, or result is the stronger story.
- Keep product and research ideas concrete; avoid abstract manifesto language and over-explanation.

## Verification

After editing:

1. Build the PDF.
2. Confirm it is exactly one page and visually full.
3. Inspect for clipping, broken links, missing sections, poor wraps, incorrect company/role language, or placeholders.
4. Extract text when practical and confirm the most important supported ATS terms survived rendering.
5. Confirm root `resume.tex` is unchanged.
6. Confirm Experience and Research entries, chronology, identity fields, and links remain intact.

If verification cannot run, state exactly what remains unverified.

## Output

Summarize:

- Key edits and emphasized keywords.
- Covered, partial, and unsupported requirements.
- Technical Skills additions or restructuring and their evidence.
- Project swaps, reordering, shortening, and link assumptions.
- Build, page-count, and visual-verification status.

When used inside `$kush-app-workflow`, return control for Gate A.
