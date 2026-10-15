---
name: kush-application-writer
description: Write Kushagra's application answers and required cover letters using his real experience and preferred voice.
---

# Kush Application Writer

Use the candidate profile, current resume, `llms.txt`, user context, and relevant primary-source company information.

- Answer the exact prompt, respect its limit, and return paste-ready prose.
- Sound direct, ambitious, technically strong, and human. Lead with the problem or product, then relevant engineering decisions, ownership, and outcomes.
- Use the strongest relevant evidence. Avoid generic praise, inflated adjectives, and mechanically repeating the resume.
- Treat confirmed accomplishments as established; do not repeatedly question ownership. Never invent metrics, experience, credentials, company facts, or personal motivations.
- Use confirmed facts for authorization, sponsorship, availability, and exact GPA; flag missing answers rather than guessing.
- Write cover letters only when required or requested. Keep them specific, concise, and at most one page, using current signature links.
- Use `$humanizer` for final prose cleanup when available and invoked, preserving facts and technical meaning.

## Files and PDF commands

Use paths relative to the Internship Applications repository root. Save requested company files under `01_Tailored_Applications/Company Name`, with answers in `application-questions.md` and a required or requested letter in `cover-letter.md`. Use `02_Portfolio_and_Visuals` for portfolio assets, `03_Interview_Prep` for preparation material, and `00_Archive` for historical work. The approved general resume remains the root `Kushagra Bharti Resume.pdf`.

When a source packet is needed, `./scripts/new-tailored-application.sh "Company Name"` copies the root templates while preserving existing company files. Replace placeholders in the company's letter before building. The local helpers require macOS and Python 3.14; use the existing launchers and dependencies described in `README.md`.

Build only the requested document, from the repository root:

```sh
# Required/requested cover letter; does not rebuild the resume.
./scripts/build-application.sh "Company Name" --skip-resume --markdown-pdf-engine helium

# Only when the user requested a company resume edit.
./scripts/build-application.sh "Company Name" --skip-cover-letter --compiler pdflatex
```

Outputs are `Kushagra Bharti Cover Letter - Company Name.pdf` and `Kushagra Bharti Resume - Company Name.pdf` inside that company's packet. Inspect the generated PDF for layout and content before using it. Do not run a build without the company argument unless updating the root documents is intended.

For a complete packet containing both PDFs with those filenames:

```sh
./scripts/verify-application.sh --company "Company Name"
```

Verification checks both PDFs and sources and renders previews into the packet's `.verify` folder. `--skip-render` skips only preview generation. This verifier requires both PDFs; for a letter-only or resume-only task, inspect the actual requested document directly rather than creating an unrequested document to satisfy it. Automated checks do not replace a visual review or grant submission permission.

Return each prompt, answer, applicable word/character count, and unresolved question. Drafting does not authorize submission.
