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

Install with:

```powershell
npx skills add https://github.com/KushagraBharti/kush-agent-skills --skill kush-app-workflow
```

For Codex global install:

```powershell
npx skills add https://github.com/KushagraBharti/kush-agent-skills --skill kush-app-workflow -g -a codex -y --copy
```

Use it with a prompt like:

```text
Use $kush-app-workflow.

Company:
<company>

Job posting:
<paste job posting or URL>

Extra notes:
<optional>
```
