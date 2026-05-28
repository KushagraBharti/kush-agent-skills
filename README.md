# Kush Agent Skills

Public skill repo for Kushagra Bharti's repeatable internship-application workflow.

The repo is intentionally small: the skills live directly under `skills/`, each with a `SKILL.md` and optional `agents/openai.yaml` metadata. The skills should give agents useful direction without over-constraining their judgment.

## Skills

```text
skills/
  kush-app-workflow/
  kush-resume-tailor/
  kush-cover-letter/
```

### kush-app-workflow

End-to-end orchestration for an internship application:

1. Create or inspect a company packet under `03_Tailored_Applications/<Company>/`.
2. Coordinate `$kush-resume-tailor`, `$kush-cover-letter`, and `$humanizer` when invoked.
3. Research the company and job from current sources.
4. Tailor the company-specific resume and cover letter without touching root templates.
5. Answer written application questions when provided.
6. Build the resume and cover-letter PDFs.
7. Verify page count, layout, placeholders, role/company names, and basic ATS coverage.
8. Stop at Gate A for human feedback or `lgtm`.
9. Commit and push approved application changes.
10. Research LinkedIn targets.
11. Draft 240-300 character outreach notes.
12. Stop at Gate B until explicit `LGTM SEND`.

Important guardrails:

- The workflow skill orchestrates; specialist skills own writing and resume judgment.
- The agent should use the job description and `llms.txt` with judgment, not rigid role buckets.
- Gate A includes compact ATS coverage: covered, partially covered, unsupported.
- LinkedIn target mix is 2 technical people, 2 hiring/recruiting people, and 1 free highest-ROI person.
- LinkedIn sends require exact `LGTM SEND`.

### kush-resume-tailor

Company-specific LaTeX resume tailoring for:

```text
03_Tailored_Applications/<Company>/resume.tex
```

The resume skill treats the resume as already full. Improvements should come from rewriting, compressing, reordering, and project swaps from `llms.txt`, not from adding net length.

Core behavior:

- Preserve root `resume.tex`.
- Keep the resume one page.
- Build a compact ATS map from the job description.
- Use exact job keywords only when supported by real evidence.
- Preserve dates, titles, metrics, links, and LaTeX syntax.
- Flag unsupported requirements instead of inventing experience.

### kush-cover-letter

Company-specific cover-letter tailoring for:

```text
03_Tailored_Applications/<Company>/cover-letter.md
```

The cover-letter skill writes concise, specific, human cover letters that connect real experience to the company and role without repeating the resume mechanically.

Core behavior:

- Preserve the root cover-letter template.
- Keep the letter one page.
- Use company/product/team facts only when researched or user-provided.
- Keep the tone direct, ambitious, technical, and normal.
- Use the standard linked signature with consecutive lines and no blank lines inside the signature block.
- Run a final humanizer-style cleanup when `$humanizer` is active.

## Recommended Project Install

For the Internship Applications workspace, install these skills project-locally, not globally:

```powershell
cd "C:\Users\kushagra\OneDrive\Documents\Internship Applications"
npx skills add https://github.com/KushagraBharti/kush-agent-skills --skill "*" -y --copy
```

Install the companion humanizer skill project-locally:

```powershell
npx skills add https://github.com/blader/humanizer --skill humanizer -y --copy
```

Restore from the project lockfile later with:

```powershell
npx skills experimental_install
```

## Prompt Template

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

## Maintenance

After changing a skill:

```powershell
python "C:\Users\kushagra\.codex\skills\.system\skill-creator\scripts\quick_validate.py" "skills\kush-app-workflow"
python "C:\Users\kushagra\.codex\skills\.system\skill-creator\scripts\quick_validate.py" "skills\kush-resume-tailor"
python "C:\Users\kushagra\.codex\skills\.system\skill-creator\scripts\quick_validate.py" "skills\kush-cover-letter"
```

Then commit and push this repo, and refresh the project-local install in the Internship Applications repo.
