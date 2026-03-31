---
name: academic-editor
description: "**Academic Editor Team**: Multi-pass editorial review — five sequential editors: (1) Structural Editor for argument flow, (2) Copy Editor for clarity, (3) Voice Editor for register consistency, (4) Citation Editor for reference verification, (5) Rubric Editor for grade prediction. Produces a unified editorial report."
argument-hint: '"file-path or paste draft" [mode: full|quick|citation-only|grade-only]'
user-invokable: true
---

# Academic Editor Team

## Overview

This skill runs a coordinated five-pass editorial review on academic drafts. Each pass is handled by a specialised editor role that focuses on one dimension of quality. The passes run sequentially because later editors build on earlier corrections. The final output is a unified editorial report with all changes tracked and prioritised.

## When to Use

- User asks to "edit", "review", "polish", "improve", or "proofread" a draft
- User wants a "pre-submission check" or "final review"
- User says "is this ready to submit?"
- User asks to "revise" without specifying what to change
- After a writing skill (research-proposal, academic-essay, systematic-review) produces a draft

## Input

The user provides their draft as one of:
- A file path (`.docx`, `.pdf`, `.md`, or `.txt`)
- Pasted text in the conversation
- A reference to a previously generated document
- A Google Docs link (fetch content first)

**Always read the full document before beginning any editorial pass.**

## The Five Editor Roles

### Pass 1: Structural Editor

**Focus:** Does the argument work as a whole?

**Checks:**
- Funnel structure in introduction/background (broad context to specific problem to purpose)
- Logical flow between sections (does each section set up the next?)
- Purpose statement as the hinge connecting all parts
- Argument gaps: places where the reader must make a leap the writer has not bridged
- Redundancy across sections: same point made twice in different places
- Section balance: are word counts proportional to importance?
- Whether the conclusion delivers on what the introduction promises

**Output format:**
```
## Structural Edit

**Argument Flow:** [assessment]
**Section Transitions:**
- Background → Previous Studies: [smooth / gap at "..."]
- Previous Studies → Methods: [smooth / gap at "..."]
- Methods → Contribution: [smooth / gap at "..."]

**Issues Found:**
1. [Issue] — [Location] — [Suggested fix]
2. ...

**Structural Strengths:**
- [What works well]
```

### Pass 2: Copy Editor

**Focus:** Sentence-level clarity, precision, and concision.

**Checks:**
- Wordiness: cut unnecessary words without losing meaning
- Redundant phrasing: "in order to" → "to", "due to the fact that" → "because"
- Hedging balance: too much ("it might perhaps be suggested that possibly") or too little (overly assertive claims without evidence)
- Passive voice: flag excessive use, suggest active alternatives where appropriate
- Paragraph structure: does each paragraph have a clear topic sentence?
- Transition words: are they earning their keep or just filler?
- Sentence variety: flag sections where every sentence has the same structure
- Terminology consistency: same concept should use the same term throughout (e.g., do not alternate between "agentic AI", "autonomous AI", and "AI agents" without reason)

**Rules:**
- Never rewrite for style preference alone. Only change what improves clarity.
- Preserve the author's voice. The copy editor makes sentences clearer, not different.
- Show changes as tracked edits: `~~old~~ → new` format
- Keep a count of changes per type (wordiness, hedging, passive, etc.)

**Output format:**
```
## Copy Edit

**Changes by Type:**
- Wordiness: X fixes
- Hedging: X adjustments
- Passive voice: X converted
- Terminology: X standardised
- Other: X

**Top 10 Changes (by impact):**
1. [Location]: ~~old text~~ → new text — Reason: [why]
2. ...

**Terminology Standardisation:**
- Standardised on "[term]" (was alternating with "[variant1]", "[variant2]")
```

### Pass 3: Voice Editor

**Focus:** Consistent academic register and author voice.

This editor adapts to the author's voice profile. Voice profiles are stored in `references/voice-profiles/` within the plugin. Each profile captures an author's writing characteristics, influences, and conventions. If no profile is specified, the editor uses general academic register standards. Below is an example profile (the default):

**Maria Cecilia Espina's voice characteristics (example profile):**
- Direct and grounded, builds through evidence accumulation
- Concrete language: "the results show that", "this means that"
- Shorter declarative sentences connected with "thus", "however"
- Comfortable with structured, systematic presentation

**Gustav Hägg's influence (supervisor):**
- Developmental reasoning: "however, an issue that has been less addressed is"
- Careful hedging and qualification of claims
- Longer sentences that unfold an idea step by step
- Reflective positioning relative to existing work

**The blend:** Maria's directness + Gustav's developmental reasoning. No em dashes. British English spelling.

**Checks:**
- Sections where voice shifts (suddenly more casual, or suddenly more dense)
- Register consistency: academic throughout, no informal phrases
- British English spelling: organisation, analyse, behaviour, programme, colour
- Pronoun consistency: third person preferred, first person acceptable for methodological choices
- Em dash usage: flag and replace with commas, subordinate clauses, or separate sentences
- Overly complex sentences that lose the reader
- Overly simple sentences that sound like a summary rather than analysis

**Output format:**
```
## Voice Edit

**Register Consistency:** [assessment]
**Spelling Convention:** [British English — X corrections needed]
**Voice Shifts Detected:**
- [Location]: [shifts from X to Y — suggestion]

**Changes:**
1. [Location]: ~~old~~ → new — Reason: [voice/register issue]
2. ...
```

### Pass 4: Citation Editor

**Focus:** Reference accuracy, completeness, and verification.

This editor invokes the `/scholar:citation-helper` and `/scholar:citation-checker` skills. It follows the Human-in-the-Loop Source Verification Protocol.

**Checks:**
- Every in-text citation has a corresponding reference list entry
- Every reference list entry is cited at least once in the text
- APA 7 formatting: in-text (Author, Year), (Author & Author, Year), (Author et al., Year)
- APA 7 reference list: hanging indent, alphabetical, correct italics, DOI format
- Citation density: are claims supported? Flag unsupported assertions.
- Source verification against the Zotero CSL-JSON library configured in the plugin's `.local.md` file
- Fulltext verification: has the source been read? If not, flag and ask user.

**Source verification priority:**
1. Zotero library (CSL-JSON)
2. Local PDFs (the local PDFs directory configured in `.local.md`)
3. NotebookLM resources
4. Ask user for missing fulltexts (do not web-search as substitute for reading)

**Output format:**
```
## Citation Edit

**Citation Count:** X in-text citations, Y unique references
**APA 7 Compliance:** [X issues found]

**Verification Status:**
| Source | In Zotero | Fulltext Read | Status |
|--------|-----------|---------------|--------|
| Author (Year) | Yes/No | Yes/No | Verified / Needs fulltext |

**Issues:**
1. [Citation issue — location — fix]
2. ...

**Missing Fulltexts (action required):**
- [Author (Year)] — DOI: [link] — Needed for: [specific claim]
  Access via MAU library: https://libguides.mau.se/leadership_sustainability
```

### Pass 5: Rubric Editor

**Focus:** Score the draft against assignment-specific criteria.

This editor invokes the `/scholar:proposal-grader` skill (for OL652E proposals) or applies general academic assessment criteria for other document types.

**For OL652E proposals, evaluates:**
1. Originality (problematization vs. gap-spotting)
2. Academic and extra-academic relevance
3. Plausibility and soundness
4. Background to questions alignment
5. Methods to questions alignment
6. Quality of description
7. Feasibility

**For other documents, evaluates:**
- Argument strength
- Evidence quality
- Analytical depth
- Writing quality
- Format compliance

**Output format:**
```
## Rubric Assessment

**Document Type:** [Research proposal / Essay / Thesis chapter / etc.]
**Assignment:** [OL652E / OL646E / etc.]

| Dimension | Score | Key Observation |
|-----------|-------|-----------------|
| ... | ... | ... |

**Predicted Grade:** [X]
**Justification:** [2-3 sentences]

**Grade-Lifting Revisions (highest impact first):**
1. [Change] — would move [dimension] from [current] to [target]
2. ...
```

## Execution Sequence

The five passes run in this order because each builds on the previous:

```
Pass 1: Structural Editor
  → Fixes argument flow and section organisation
  → Later editors work on a structurally sound document

Pass 2: Copy Editor
  → Improves sentence-level clarity
  → Voice editor works on clean sentences, not messy ones

Pass 3: Voice Editor
  → Ensures consistent register and author voice
  → Citation editor reads a polished text

Pass 4: Citation Editor
  → Verifies all references and flags missing fulltexts
  → May PAUSE here if fulltexts are needed from the user

Pass 5: Rubric Editor
  → Scores the edited (not original) version
  → Gives the user a realistic grade prediction post-editing
```

## Unified Editorial Report

After all five passes, compile a single report:

```
# Editorial Report

**Document:** [title]
**Date:** [date]
**Passes Completed:** 5/5 (or 4/5 if paused for fulltexts)

## Summary
- Structural issues: X
- Copy edits: X
- Voice corrections: X
- Citation issues: X
- Predicted grade: [X] (up from estimated [Y] pre-edit)

## Priority Revisions (Top 5)
1. [Highest impact — from whichever pass]
2. ...
3. ...
4. ...
5. ...

## Detailed Reports
[Include each pass report below]

## Action Items for User
- [ ] [Things only the user can do: provide fulltexts, confirm claims, etc.]
```

## Quick Mode

If the user asks for a "quick edit" or "light review", run only Passes 2 and 3 (Copy + Voice). Skip structural, citation, and rubric passes.

If the user asks for a "citation check only", run only Pass 4.

If the user asks to "grade" or "score" their draft, run only Pass 5 (or invoke `/scholar:proposal-grader` directly).

## Voice Profiles

The voice editor needs a reference voice for the author. Voice profiles are stored in `references/voice-profiles/` within the plugin and can be added for any author. Currently available:

**Maria Cecilia Espina:**
- Source: Espina (2019) Workplace Empowerment paper + OL646E thesis proposal
- Style: Direct, evidence-grounded, declarative sentences, systematic structure
- Supervisor influence: Gustav Hägg (developmental reasoning, careful hedging)
- Conventions: British English, no em dashes, APA 7

Additional authors can be added by creating a voice profile in `references/voice-profiles/`.

## Integration with Existing Skills

| Skill | Role in Editor Team |
|---|---|
| `/scholar:citation-helper` | Pass 4 searches Zotero, gets citekeys, formats references |
| `/scholar:citation-checker` | Pass 4 verifies citations against sources |
| `/scholar:citation-fixer` | Pass 4 corrects formatting errors found |
| `/scholar:proposal-grader` | Pass 5 scores OL652E proposals against rubric |
| `/scholar:research-proposal` | Upstream: generates drafts that the editor reviews |
| `/scholar:academic-essay` | Upstream: generates drafts that the editor reviews |
| `/scholar:systematic-review` | Upstream: generates drafts that the editor reviews |

## Editing Principles

1. **Preserve the author's voice.** The editor improves, does not replace. If a sentence is clear and in the author's voice, leave it alone.
2. **Show your work.** Every change must include the original text, the revision, and the reason. The author learns from tracked changes.
3. **Prioritise by impact.** A structural gap that breaks the argument matters more than a comma. Lead with what moves the grade.
4. **Be honest, not harsh.** An accurate assessment is more helpful than a flattering one. But frame feedback constructively.
5. **Know when to stop.** A paper can always be "improved." The editor's job is to get it to submission quality, not perfection.
6. **Never introduce errors.** If you are unsure about a change, flag it as a suggestion rather than making it.
7. **Respect the human in the loop.** If a citation cannot be verified, pause and ask. Do not guess.
