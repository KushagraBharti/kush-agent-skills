# Kush Agent Skills

Public Agent Skills for repeatable personal workflows.

## Skills

### kush-app-workflow

End-to-end internship application workflow:

1. Create a company-specific application packet.
2. Tailor the company copy of `resume.tex`.
3. Tailor the company copy of `cover-letter.md`.
4. Build resume and cover letter PDFs.
5. Verify page counts and layout.
6. Wait for human approval.
7. Commit and push approved changes.
8. Research LinkedIn connection targets.
9. Draft outreach notes.
10. Send notes only after explicit approval.

### kush-resume-tailor

Kush-specific resume tailoring for the company copy of `resume.tex`. Keeps root materials untouched, preserves factual claims, and optimizes for the target job while protecting the one-page layout.

### kush-cover-letter

Kush-specific cover letter drafting for the company copy of `cover-letter.md`. Uses `llms.txt`, the resume, the job posting, and company research to write a specific one-page letter.

Install with:

```powershell
npx skills add https://github.com/KushagraBharti/kush-agent-skills --skill "*"
```

For Codex global install:

```powershell
npx skills add https://github.com/KushagraBharti/kush-agent-skills --skill "*" -g -a codex -y --copy
```

Install the humanizer skill separately if you want the full four-skill workflow:

```powershell
npx skills add https://github.com/blader/humanizer --skill humanizer
```

Use it with a prompt like:

```text
Use $humanizer, $kush-resume-tailor, $kush-cover-letter, and $kush-app-workflow.

Company:
<company>

Job posting:
<paste job posting or URL>

Extra notes:
<optional>
```
