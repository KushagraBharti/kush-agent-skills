---
name: kush-resume-tailor
description: Tailor Kushagra's company-specific LaTeX resume for a job posting while preserving truth, one-page layout, and the root resume template. Use when optimizing the company folder resume.tex under 03_Tailored_Applications for ATS alignment, role-specific emphasis, keyword coverage, and concise resume edits inside the application workflow.
---

# Kush Resume Tailor

## Purpose

Optimize the company-specific `resume.tex` for one target job while preserving the user's actual experience, writing style, and one-page resume constraint.

Use this skill inside `$kush-app-workflow` or on its own when the user asks for resume tailoring. If `$humanizer` is active, do not use it on the resume unless the user explicitly asks; resume bullets should stay precise and compact.

## Required Context

Read:

- The job posting or job URL.
- `llms.txt` for current profile context.
- Root `resume.tex` for baseline context only.
- Company-specific `03_Tailored_Applications/<Company>/resume.tex` for edits.

Hard rule: never edit root `resume.tex`. Only edit the company-specific copy.

## Tailoring Process

1. Identify the target role type: SWE, AI/ML, full-stack, backend, data, research, finance/quant, product-adjacent, or other.
2. Extract the job's real requirements: technologies, responsibilities, seniority signals, domain, and soft skills.
3. Map requirements to existing evidence in `llms.txt` and the resume.
4. Edit the company-specific LaTeX resume with the smallest changes that materially improve fit.
5. Preserve one-page layout and compile-readiness.

## Editing Rules

Do:

- Reorder bullets or skill terms when the role clearly prioritizes them.
- Swap weak verbs for concrete technical verbs.
- Add exact job keywords only when supported by existing experience.
- Emphasize the most relevant projects and experiences.
- Keep metrics, tools, systems, and outcomes specific.
- Prefer tight bullet edits over new sections.
- Preserve LaTeX syntax, links, commands, spacing, and section structure.

Do not:

- Invent experience, metrics, titles, dates, tools, awards, publications, GPA, certifications, or company names.
- Edit the root resume template.
- Add generic ATS stuffing.
- Add claims that only appear in the job description but not in the user's materials.
- Make broad style rewrites when a targeted keyword or bullet edit would work.
- Expand the resume beyond one page.

## ATS Guidance

Prefer exact terms from the job posting when truthful:

- Languages and frameworks.
- Cloud/database/tool names.
- ML/AI systems, data pipelines, evaluation, full-stack, backend, distributed systems, APIs, automation, testing, deployment, observability.
- Role nouns such as intern, software engineer, machine learning, research, product engineering, infrastructure.

Include both acronym and expanded form only when space allows and it is useful, for example `LLM` and `large language model`.

## Bullet Standard

Strong bullets should usually follow:

```text
Action + technical work + concrete outcome/scale + tool/system/context
```

Example shape:

```text
Built <system/pipeline> using <tools> to <measurable/resulting impact>.
```

Do not force metrics where none exist. A truthful technical detail is better than a fake number.

## Output Expectations

When finished, summarize:

- Key resume changes.
- Job keywords added or emphasized.
- Any important requirement not supported by the current materials.
- Whether the resume still needs build/page verification.

If used inside `$kush-app-workflow`, return control to that workflow so it can build and verify PDFs.
