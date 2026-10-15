---
name: kush-app-workflow
description: Manage Kushagra's application batches, candidate context, dashboard review, submission tracking, and outreach.
---

# Kush App Workflow

Prepare applications, send them to https://employment.kushagrabharti.com for review, then act on saved decisions.

- Use the current general resume unchanged. Read `candidate-profile.md` if available, the resume, `llms.txt`, and user instructions. Keep private facts in the application workspace, not this public skills repo.
- Use supplied job links. For company-only requests, select suitable live roles using the candidate's preferences. Track each posting separately and check for duplicates.
- Use `$kush-application-writer` for answers and required letters; use `$kush-browser-apply` for forms and submission. Separate agents are not required.
- Ask only for missing facts that affect an answer or eligibility. Continue other applications while waiting.
- Approve authorizes the reviewed application; Edit requires changes and fresh review; Cancel stops it. Only confirmed submission counts as applied.
- On resuming, read the dashboard queue. While awaiting review, save progress and stop. These skills do not install a listener or automatically wake Codex.
- Keep outreach separate from application approval: research two technical contacts, two hiring contacts, and one additional strong contact per company. Draft notes within the platform's limit; send only after `LGTM SEND` for the reviewed recipients and messages.
- Locate the Internship Applications workspace from the current project context and its root documents. The canonical `kush-agent-skills` checkout is not the application workspace. Do not assume a fixed home directory. Preserve root documents and unrelated work. The application helpers support macOS and Python 3.14 only; do not use removed PowerShell or batch launchers on another host.

## Workspace and local scripts

Paths below are relative to the Internship Applications repository root.

| Folder | Contents |
| --- | --- |
| `00_Archive` | Archived applications and `Hone Archive`. |
| `01_Tailored_Applications` | Current company packets and application answers. |
| `02_Portfolio_and_Visuals` | Portfolio documents and images. |
| `03_Interview_Prep` | Interview notes and preparation materials. |

The current general resume is `Kushagra Bharti Resume.pdf` at the root. The root also contains `resume.tex`, `Kushagra Bharti Cover Letter - General Template.md`, `Kushagra Bharti Transcript.pdf`, `llms.txt`, and `Application_Tracker.xlsx`. Use the existing `scripts/` folder. See `README.md` for dependencies and available flags.

Run these from the repository root, quoting names and paths with spaces:

```sh
./scripts/new-tailored-application.sh "Company Name"
./scripts/cleanup-application.sh "Company Name"
```

- Create a packet only when local files are needed. Creation copies both source templates into `01_Tailored_Applications/Company Name` and preserves existing sources. Do not use `--force` unless replacing those sources is intended. A copied cover-letter template does not mean a cover letter is required.
- Use `$kush-application-writer` for the appropriate build and verification commands. Keep the approved general resume unchanged unless the user requests a resume edit.
- Cleanup removes only `.build`, `.verify`, `verify`, `render`, and `renders` inside that packet. Keep answers, sources, final PDFs, and submission evidence.
- The shell launchers choose Python through `scripts/run-python.sh`. Use `APPLICATION_PYTHON` to override the interpreter when needed; it must be Python 3.14. Do not create another environment or script directory for routine use.

After a confirmed submission, the existing tracker helper can record it:

```sh
./scripts/update-tracker.sh --company "Company Name" --role "Role Title" \
  --role-type Internship --status applied --posting-link "https://example.com/job"
```

The helper updates `Application_Tracker.xlsx` by company, not by posting. Check the existing row before replacing it; keep separate posting IDs and decisions in the dashboard. Always pass the intended `--status`. Its defaults are `applied` and today's date, so supply `--date-applied` with the actual date when recording an earlier submission. It cannot leave the application date blank; keep unsubmitted progress in the dashboard rather than assigning a false application date. Use `--tracker` only when an alternate workbook is intended.

Return the dashboard link and a short summary of ready, submitted, cancelled, and blocked applications.
