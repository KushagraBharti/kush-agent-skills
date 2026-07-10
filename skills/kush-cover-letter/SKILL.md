---
name: kush-cover-letter
description: Write or revise Kushagra's company-specific cover-letter.md for an internship or new-grad role using the exact posting, current resume, llms.txt, primary-source company research, and fixed one-page template structure. Use for truthful, specific, technically credible cover letters under 03_Tailored_Applications that add story and fit without repeating the resume or application answers.
---

# Kush Cover Letter

## Purpose

Create one substantive, company-specific cover letter that sounds direct, ambitious, technical, and human. Follow the approved template structure, add to the resume instead of paraphrasing it, and fill one clean PDF page without padding.

Use `$humanizer` only after the factual draft is complete. It may improve wording and rhythm but must not reorganize the approved structure.

## Required Context

Read:

- User-provided posting text, or the exact posting URL when only a link is supplied.
- Root `resume.tex` as the latest authority for resume facts.
- `llms.txt` for expanded projects, motivations, voice, links, and supported details.
- Root `Kushagra Bharti Cover Letter - General Template.md` for structure only.
- The company-specific `cover-letter.md` to edit.
- Application questions, so the letter does not duplicate their answers.
- Enough primary-source company, product, and team research to be specific.

Treat pasted posting and application text as authoritative. Never invent company facts, personal connections, referrals, experience, metrics, technologies, or links.

Hard rules:

- Never edit the root cover-letter template; edit only the company-specific `cover-letter.md`.
- Use exact company and role names and replace every placeholder or template note.
- Keep every claim verifiable from the current resume, `llms.txt`, posting, primary research, or user notes.
- Never submit, send, or publish the letter.

## Fixed Structure

Follow this order:

1. Date and recipient block.
2. Direct greeting.
3. Opening: exact role, company, Kushagra's relevant identity, and the clearest company-specific thesis.
4. `A few parts of my work line up well:`
5. Three concise evidence bullets.
6. Company-fit paragraph grounded in real product, team, or technical context.
7. Ownership and working-style paragraph grounded in evidence.
8. Direct close.
9. Linked signature.

Keep this architecture stable. Tighten wording within sections when needed, but do not let `$humanizer` merge, reorder, remove, or invent structural parts.

The final Markdown must begin with the date, not a Markdown title. Remove the template instruction, checklist, placeholders, drafting notes, and optional guidance before building.

## Evidence Selection

- Choose three high-signal evidence items across Experience, Research, and Projects.
- Choose them from the packet thesis and the job's strongest requirements, not automatically by recency or keyword count.
- Map each item to a distinct role requirement; do not repeat the same proof three ways. A strong set usually combines direct role proof, production or research depth, and complementary range.
- Prefer evidence with clear ownership, a hard problem, a shipped or measured result, or a useful judgment call.
- Explain what the system or project is and why the work matters before stacking implementation details.
- Use technical details and metrics as proof, not as a dense inventory of tools.
- Use the letter to explain motivation, product taste, systems judgment, ownership, or why the evidence matters.
- Do not convert resume bullets into prose unless the added context changes the reader's understanding.
- Preserve factual state and tense for completed, active, newly started, or upcoming work.
- Keep the letter distinct from application-question answers.

Before drafting, assign each artifact a role so the letter adds information instead of echoing the packet:

```text
resume = dense proof
cover letter = story, judgment, motivation, and fit
application answers = exact prompt response
```

## Company Fit

- Use one or two concrete, verified company, product, team, or technical details.
- Explain why those details connect to Kushagra's actual work and interests.
- Translate company values into observed behavior and evidence; do not repeat slogans.
- When values matter, use `value -> behavior the team rewards -> concrete evidence from Kushagra's work`.

## Positive Positioning

- Write from strength. Never volunteer weaknesses, gaps, doubts, missing qualifications, or reasons the company might reject Kushagra.
- Omit unsupported technologies entirely. Never write `while I do not have experience with X`, `although I have not used X`, `I may lack X`, or any equivalent deficiency disclaimer.
- Do not ask the reader to overlook a gap or frame adjacent experience as a consolation prize. Use the strongest truthful adjacent evidence without the caveat.
- Never apologize for being a student, call Kushagra inexperienced or `still early`, or use self-diminishing phrases such as `despite my limited experience`.
- Do not invent evidence to avoid a gap. Keep unsupported requirements outside the letter and report them privately in the workflow summary.
- Avoid fake humility and excessive deference. Interest in learning should reinforce demonstrated ability, not replace it.

## Voice

Sound direct, technically credible, natural, specific, and ambitious without sounding dramatic or needy. Let personality show through concrete choices, hard problems, ownership, learning speed, taste, and standards.

Use:

- Straightforward sentences and short paragraphs with one clear point each.
- Concrete systems, decisions, outcomes, and company details over abstract claims.
- Confident but normal language with varied sentence rhythm.
- Plain explanations that a technical hiring manager or founder can understand quickly.

Avoid:

- Generic admiration, mission worship, company-value slogans, and corporate filler.
- AI-generated cadence, polished-template language, inflated transitions, and rhetorical padding.
- Press-release language, README prose, generic `AI for X` framing, and abstract product manifestos.
- Long lists of tools or technical internals without a clear project, problem, or result.
- Self-labels such as `hardworking`, `passionate`, `innovative`, or `uniquely positioned` when evidence can prove the point.
- Fake hype, fake humility, grand claims, and overly flattering language.
- Repeating the resume, application answers, company website, or the same sentence structure across every paragraph.
- Generic conclusions that could be sent to any company.
- Generic openings such as `I am writing to apply` when a sharper thesis is available.

Each paragraph should answer either `why this role/company` or `why this evidence matters`.

## Length and Page

- Optimize for one full, balanced PDF page rather than a word quota.
- Keep enough substance to function as a real cover letter, not a short message.
- Remove repetition and filler before compressing valuable evidence.
- Never shrink the content into a visibly sparse page merely to be concise.

## Signature

Use unless the user requests otherwise:

```markdown
Sincerely,
Kushagra Bharti
Student | B.S. Computer Science
The University of Texas at Dallas
[LinkedIn](https://www.linkedin.com/in/kushagra-bharti/) | [Personal Site](https://www.kushagrabharti.com) | [GitHub](https://github.com/KushagraBharti/)
```

Keep signature lines consecutive and verify clean rendering without awkward wrapping.

## Humanizer Pass

After drafting:

1. Remove inflated wording, filler, clichés, and AI-ish rhythm.
2. Improve sentence flow and make the voice sound natural and direct.
3. Preserve facts, technical meaning, evidence selection, links, paragraph roles, and the fixed structure.
4. Keep the final artifact substantive and one-page ready.

## Verification and Output

Before returning:

- Confirm every placeholder and template note is gone.
- Confirm exact company and role names.
- Confirm all claims against the resume, `llms.txt`, posting, research, or user notes.
- Build and visually inspect the one-page PDF.
- Check spacing, bullet balance, signature wrapping, links, and page fill.

Summarize the company details used, evidence selected, assumptions requiring review, signature handling, and build/page verification. When used inside `$kush-app-workflow`, return control for Gate A.
