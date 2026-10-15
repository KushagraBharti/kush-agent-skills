# Kush Agent Skills

Three small skills: prepare a batch, review it from anywhere, then submit approved applications.

| Skill | Responsibility |
| --- | --- |
| `kush-app-workflow` | Queue, candidate context, review decisions, tracking, and outreach |
| `kush-application-writer` | Application answers and required cover letters |
| `kush-browser-apply` | Forms, uploads, readbacks, approved submission, and confirmation |

Use the existing general resume unchanged. Cover letters are written only when required or requested. Only confirmed submissions count as applied.

Review happens at [employment.kushagrabharti.com](https://employment.kushagrabharti.com). The browser skill uses the existing `scripts/employment-client.mjs` in the Personal-Site repository. Keep candidate information and credentials private. These skills do not install a background agent or an approval-triggered wake-up service.

## Install

Install from the Internship Applications workspace on macOS or Windows. Skill installation is cross-platform; the workspace's local application helpers require macOS and Python 3.14.

```sh
npx skills add https://github.com/KushagraBharti/kush-agent-skills --skill '*' -y --copy
```

Install project-locally. When replacing the old set, remove only the old project-local `kush-app-workflow`, `kush-resume-tailor`, and `kush-cover-letter` directories first so stale instructions do not remain.

The Codex skill-installer also supports a pinned download:

```text
python <skill-installer>/scripts/install-skill-from-github.py --repo KushagraBharti/kush-agent-skills --ref <commit> --path skills/kush-app-workflow skills/kush-application-writer skills/kush-browser-apply --dest .agents/skills
```

## Use

```text
Use $kush-app-workflow, $kush-application-writer, and $kush-browser-apply.
Prepare these jobs for dashboard review without submitting:
<companies or job links>
```

## Maintain

The `skills/` directory in this repository is the source of truth. The Internship Applications workspace's `.agents/skills/` directories are installed copies. Make future edits here, validate each folder with the skill-creator's `scripts/quick_validate.py`, commit and push this repository, then refresh the installed copies. If a workspace copy is edited first, bring those changes back here before refreshing it. Keep the skills short.
