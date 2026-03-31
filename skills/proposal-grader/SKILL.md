---
name: proposal-grader
description: "**Research Proposal Grader**: Evaluates research proposal drafts against a rubric. Scores seven dimensions (originality, relevance, soundness, alignment, quality, feasibility), predicts grade A-F, and gives actionable revision priorities. Adaptable to any research methods course rubric."
argument-hint: '"file-path or paste draft" [rubric: default|custom]'
user-invokable: true
---

# Research Proposal Grader

## Overview

This skill evaluates a research proposal draft against a research proposal rubric (default rubric based on Hug & Aeschbach, 2020; originally used in OL652E). It produces a structured assessment report with a predicted grade and specific revision guidance to improve the score before submission. The default rubric can be adapted to match any research methods course rubric.

## When to Use

- User asks to "grade", "check", "review", "assess", or "evaluate" a research proposal
- User wants to know what grade their draft would receive
- User asks "is this good enough" or "what should I improve"
- User wants feedback before submitting

## Input

The user provides their draft as one of:
- A file path (`.docx`, `.pdf`, `.md`, or `.txt`)
- Pasted text in the conversation
- A reference to a previously generated proposal

Read the full draft before evaluating. Never assess based on partial content.

## Evaluation Process

### Step 1: Structural Completeness Check

Verify all five required sections are present and within word targets:

| Section | Required | Word Target |
|---------|----------|-------------|
| a) Background and problem discussion | Yes | 500–750 |
| b) Previous studies (4–6 articles) | Yes | ~500 |
| c) Methods and research questions | Yes | ~500 |
| d) Practical contribution | Yes | 250–500 |
| e) Reference list | Yes | n/a |

**Total body text target:** 1,800–2,200 words (excluding references, figures, diagrams)

Flag any missing sections or significant word count deviations.

### Step 2: Score Each Dimension

Evaluate the draft on each of the seven rubric dimensions. For each dimension, assign one of:

- **Excellent** — exceeds expectations, A-level quality
- **Good** — solid, meets B-level expectations
- **Adequate** — meets minimum C-level requirements
- **Weak** — below expectations, D–E level
- **Missing/Failing** — absent or fundamentally unclear

#### Dimension 1: Originality
- Is the research idea novel?
- Does it engage with the research frontier (current debates, emerging questions)?
- Or does it merely replicate existing work / state the obvious?
- Does it use **problematization** (challenging assumptions) rather than just **gap-spotting** ("no one has studied X")? Per Sandberg & Alvesson (2011), problematization is valued higher.
- Per Hug & Aeschbach (2020): originality is assessed on the *aims and outcomes*, not on methods. A well-known method applied to a novel question is still original.

#### Dimension 2: Academic and Extra-Academic Relevance
- **Scientific value (academic relevance)**: Does it clearly contribute to scholarly understanding in leadership/organisation/sustainability?
- **Societal value (extra-academic relevance)**: Does it connect to real-world issues, SDGs, or stakeholder needs?
- Is the "so what?" convincingly answered?
- Per Hug & Aeschbach (2020): these are **equally weighted** -- societal relevance is not a secondary add-on. SDG connections and section (d) carry real grading weight.

#### Dimension 3: Plausibility and Soundness
- **Coherence**: Do the sections form one integrated argument (not five disconnected parts)?
- **Justifiability**: Are theoretical and methodological choices explained and defended?
- **Appropriateness**: Do the chosen theories and methods fit the research problem?

#### Dimension 4: Background ↔ Questions Alignment
- Does the background discussion logically lead to the stated purpose?
- Do the research questions directly address the problem identified?
- Is there an unbroken argument chain: problem → gap/tension → purpose → RQs?

#### Dimension 5: Methods ↔ Questions Alignment
- Do the proposed qualitative methods fit the qualitative RQs?
- Do the proposed quantitative methods fit the quantitative RQs?
- Are data gathering and analysis methods appropriate for each perspective?
- Are all four RQs (2 qual + 2 quant) present and aligned with the purpose?
- **Critical**: The rubric asks to show how the topic *could be* studied from both perspectives (methodological literacy), NOT to propose a mixed-methods study. Conditional language ("would", "could") is appropriate. Proposing an integrated mixed-methods design is overreach unless explicitly justified.

#### Dimension 6: Quality of Description
- **Clarity**: Is the writing clear, precise, and free of ambiguity?
- **Completeness**: Are all required sub-elements addressed within each section?
- **Structure**: Are headings, paragraphs, and transitions well-organised?
- **Citations**: Is APA 7 used correctly? Are citations sufficient in density?
- **Language**: Academic tone, appropriate hedging, consistent spelling conventions?

#### Dimension 7: Feasibility
- Does the author show awareness of what the research would require?
- Is the research plan/timeline realistic?
- Is data accessibility considered?
- Are the researcher's own capabilities acknowledged?
- Per Hug & Aeschbach (2020): feasibility is a **substantive criterion, not a checkbox**. An overambitious design (e.g., full mixed-methods with 18 interviews + 200 survey respondents for a master's thesis) actively scores lower than a simpler design the researcher can realistically execute. Proposing more than can be achieved demonstrates lack of resource awareness, not ambition.

### Step 3: Determine Overall Grade

Map the dimension scores to the grade descriptors:

| Grade | Requirements |
|-------|-------------|
| **A** | Most dimensions at Excellent. Original idea at the research frontier. Exceptional flow across all sections. Near-flawless writing. All B criteria superseded. |
| **B** | Most dimensions at Good or above. Original idea, well-explained relevance. Well-illustrated theory–question–method links. Very well-structured. All C criteria met. |
| **C** | All dimensions at least Adequate. Clear background–RQ link. Feasible plan. Fair methodological understanding. Relevant literature cited. All sections present and structured. |
| **D–E** | Some dimensions Weak but idea is relevant and clear. Key sections present. Course learning goals demonstrated. |
| **F** | Multiple dimensions Missing/Failing. Significant parts incomplete or major lack of clarity. |

### Step 4: Generate Revision Suggestions

For each dimension scored below Excellent, provide **specific, actionable** suggestions. Each suggestion must:
- Reference the exact part of the draft that needs work
- Explain what is wrong or missing
- Describe concretely what to do to improve it
- Indicate how much the change would affect the grade

Prioritise suggestions by impact: list the changes that would most improve the grade first.

## Output Format

Present the assessment as a structured report:

```
## Research Proposal Assessment

### Structural Check
- [ ] Section a) Background & problem: ✓/✗ (~XXX words)
- [ ] Section b) Previous studies: ✓/✗ (~XXX words, X articles reviewed)
- [ ] Section c) Methods & RQs: ✓/✗ (~XXX words, X qual RQs, X quant RQs)
- [ ] Section d) Practical contribution: ✓/✗ (~XXX words)
- [ ] Section e) References: ✓/✗ (X references)
- [ ] Total word count: ~XXXX (target: 1,800–2,200)

### Dimension Scores

| Dimension | Score | Key Observation |
|-----------|-------|-----------------|
| Originality | _____ | ... |
| Academic & Extra-Academic Relevance | _____ | ... |
| Plausibility & Soundness | _____ | ... |
| Background ↔ Questions Alignment | _____ | ... |
| Methods ↔ Questions Alignment | _____ | ... |
| Quality of Description | _____ | ... |
| Feasibility | _____ | ... |

### Predicted Grade: X

**Justification:** [2–3 sentences explaining the grade based on the rubric descriptors]

### Top Revision Priorities

1. **[Highest impact change]**
   - Where: [section/paragraph]
   - Issue: [what's wrong]
   - Fix: [what to do]
   - Impact: [how this affects the grade]

2. **[Second highest impact]**
   ...

3. **[Third highest impact]**
   ...

### Strengths
- [What the draft does well — reinforce these]

### Additional Notes
- [Minor issues, formatting, citation fixes, etc.]
```

## Hug & Aeschbach Framework: Key Grading Principles

The rubric criteria come from Hug & Aeschbach's (2020) systematic review of grant application assessment. Their framework distinguishes between **evaluated entities** (what is being assessed) and **evaluation criteria** (the dimension of assessment). This means a single proposal section is evaluated on multiple criteria simultaneously:

| Evaluated entity | Criteria applied | Proposal section |
|-----------------|-----------------|-----------------|
| Research idea / aims / outcomes | Originality, Academic relevance, Extra-academic relevance | (a) |
| Proposed research process | Quality/rigor, Appropriateness, Coherence | (c) |
| Description of the proposal | Clarity, Completeness | All sections |
| Resources for implementation | Feasibility | (c), research plan |

**Strategic implications for grading:**
1. **Coherence beats complexity** -- a simple, well-aligned proposal scores higher than an ambitious but fragmented one
2. **Feasibility penalises overreach** -- proposing more than can be achieved is a grading liability
3. **Section (c) tests methodological literacy** -- showing both perspectives, not committing to execute both
4. **Section (b) must review articles by method** -- RQs, scope, methods, data, results for each; balance qual + quant studies
5. **Extra-academic relevance carries equal weight** -- SDG links and stakeholder value are graded, not decorative

## Evaluation Principles

- **Be honest but constructive.** The goal is to help improve the draft, not to discourage. But do not inflate scores -- an honest C is more useful than a false B.
- **Grade against the rubric, not your own standards.** Use the rubric criteria from Hug & Aeschbach (2020), not general academic quality expectations. When a custom rubric is provided, use that instead.
- **Assess what is written, not what was intended.** If an idea is good but poorly expressed, that affects Quality of Description, not Originality.
- **Consider the assignment context.** This is a 2,000-word proposal for a master's methods course, not a funded research grant. Calibrate expectations accordingly.
- **Distinguish between structural and substantive issues.** Missing a section entirely (structural) is more damaging than having a weak argument in a present section (substantive).
- **Check entity-criterion alignment.** When scoring, verify you are applying the right criterion to the right entity. Originality applies to the research idea, not to the methods. Feasibility applies to the resources, not to the writing quality.

## Reference Files

- See [grading-rubric.md](references/grading-rubric.md) for the complete rubric with grade descriptors from the assignment brief.
