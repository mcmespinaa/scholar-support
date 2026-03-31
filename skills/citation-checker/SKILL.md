---
name: citation-checker
description: "**Citation Checker & Verification Agent**: Deploys 5 parallel agents to audit every citation — cross-references, APA 7 formatting, DOI validation, claim grounding, and completeness. Detects fabricated references. Produces a structured verification report."
argument-hint: '"file-path or paste document" [style: apa7]'
user-invokable: true
---

# Citation Checker & Verification Agent

## Overview

This skill deploys parallel agents to systematically verify every citation in an academic document. It catches fabricated references, misattributed claims, formatting errors, and missing sources — the most common and most damaging errors in academic writing.

## When to Use This Skill

Use when the user asks you to:
- Check or verify citations in an essay, paper, or thesis
- Audit a reference list for accuracy
- Find hallucinated or fabricated references
- Validate that claims are properly attributed to sources
- Check APA 7 formatting of citations and references
- Cross-reference in-text citations against a reference list
- Verify DOIs or publication details

## Input Requirements

The skill needs at minimum:
1. **The document to check** — a file path (`.docx`, `.md`, `.txt`, `.pdf`) or pasted text
2. **The reference list** — either embedded in the document or provided separately

Optional:
- **Source PDFs or articles** — for deep claim-vs-source verification
- **Target citation style** — defaults to APA 7

## Source Lookup Priority

Agents that verify references or suggest sources follow a **4-tier lookup chain**, checking the researcher's own sources before web searching:

```
0. ZOTERO: Search the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) by author, title, year
   ├── Match found → Use structured metadata (authors, DOI, volume, pages) as ground truth
   └── No match ↓
1. LOCAL PDFs: Search the local PDFs directory configured in `.local.md` by author surname, title keywords, or topic
   ├── Match found → Read PDF to verify content or extract metadata
   └── No match ↓
2. NOTEBOOKLM: Query researcher's NotebookLM notebooks (if available)
   ├── Match found → Use notebook content for verification
   └── No match or not configured ↓
3. WEB SEARCH: Search via WebSearch/WebFetch (fallback)
```

**Why this order:**
- **Zotero first:** The CSL-JSON file has structured, authoritative metadata imported from publisher databases. Fastest to query (JSON search vs. PDF reading). Ideal for metadata verification (authors, DOI, volume, pages) and for confirming a reference is real.
- **Local PDFs second:** For claim grounding, the full text of the paper is needed. PDFs in the local PDFs directory configured in `.local.md` provide this. Also useful when a reference isn't in Zotero but the researcher has the paper from course materials.
- **NotebookLM third:** Researcher's annotated notebooks may contain sources not yet added to Zotero or the local PDFs directory.
- **Web search last:** Only for sources not available through any local channel.

**Zotero lookup:** Read and parse the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`). Search by author `family` name, `title` keywords, or `issued` year. Use the `citation-helper` skill's search logic (Operation 1). If the reference is found in Zotero, its CSL-JSON metadata is authoritative — use it to verify or correct authors, DOI, volume, issue, pages.

**Local source matching:** Search the local PDFs directory configured in `.local.md` using Glob for PDF filenames containing the author surname or title keywords (e.g., `**/*Silvius*Schipper*.pdf`, `**/*complexity*leadership*.pdf`). See `references/verification-patterns.md` for matching patterns.

**NotebookLM integration:** If the researcher has set up NotebookLM notebooks with collected sources, query them via `/notebooklm` before falling back to web search. This is optional — if no notebooks are configured, skip gracefully.

## Agent Architecture

The skill deploys **5 parallel verification agents**, each handling one class of checks. All agents run concurrently via the Agent tool and report back to a coordinator that merges findings into a single report.

```
                    ┌─────────────────────┐
                    │   COORDINATOR       │
                    │   (this skill)      │
                    └────────┬────────────┘
                             │ launches 5 agents in parallel
         ┌───────────┬───────┼───────┬────────────┐
         ▼           ▼       ▼       ▼            ▼
    ┌─────────┐ ┌─────────┐ ┌────┐ ┌──────┐ ┌──────────┐
    │ Agent 1 │ │ Agent 2 │ │ A3 │ │  A4  │ │ Agent 5  │
    │ Cross-  │ │ Format  │ │DOI │ │Claim │ │ Orphan & │
    │ Ref     │ │ Check   │ │Val │ │Ground│ │ Ghost    │
    └─────────┘ └─────────┘ └────┘ └──────┘ └──────────┘
```

### Agent 1: Cross-Reference Checker

**Purpose:** Ensure every in-text citation has a matching reference list entry, and every reference list entry is cited at least once.

**Checks:**
- Extract all in-text citations: `(Author, Year)`, `(Author & Author, Year)`, `(Author et al., Year)`, narrative forms like `Author (Year)`
- Extract all reference list entries
- Match each in-text citation to a reference list entry
- Flag in-text citations with no matching reference (ghost citations)
- Flag reference list entries that are never cited in the text (orphan references)
- Check for author name spelling consistency between in-text and reference list
- Check year consistency between in-text and reference list

**Output fields per issue:**
| Field | Description |
|-------|-------------|
| Location | Page/paragraph/line where the issue appears |
| Citation | The problematic citation text |
| Issue | What's wrong |
| Severity | Error / Warning |
| Suggestion | How to fix it |

### Agent 2: APA 7 Format Checker

**Purpose:** Verify that all citations and references follow APA 7 formatting rules.

**In-text citation checks:**
- Correct use of `&` (parenthetical) vs. `and` (narrative)
- `et al.` used correctly for 3+ authors from first citation
- Page numbers present for direct quotes
- Multiple sources within parentheses are alphabetical and semicolon-separated
- Organisation abbreviations introduced correctly: first use `(Organisation [ABBR], Year)`, subsequent `(ABBR, Year)`
- Year present in every citation
- Correct punctuation placement (period after parenthetical citation, not before)

**Reference list checks:**
- Hanging indent formatting (if checking .docx structure)
- Alphabetical ordering by first author surname
- Author name format: `Surname, A. A.`
- Year in parentheses after authors
- Journal name and volume italicised
- Issue number in parentheses, not italicised
- DOI as `https://doi.org/` hyperlink (not `doi:` or `dx.doi.org`)
- Book titles italicised
- Capitalisation: sentence case for article titles, title case for journal names
- Edition notation: `(2nd ed.)` not `(Second edition)`
- `In` before editor names in book chapters
- `(Ed.)` / `(Eds.)` formatting
- No "Retrieved from" before URLs
- Consistent double-spacing

**Output fields per issue:**
| Field | Description |
|-------|-------------|
| Location | Position in document |
| Current | What's currently written |
| Issue | Which APA 7 rule is violated |
| Severity | Error / Warning / Style |
| Fix | Corrected version |

### Agent 3: DOI & Publication Validator

**Purpose:** Verify that references point to real publications. This is the primary defence against hallucinated references.

**Checks:**
- If DOI is present: validate format (`10.XXXX/YYYY`)
- If DOI is present: attempt to resolve it via web search to confirm it exists and matches the cited article
- Verify author names, year, title, and journal match a real publication (via web search)
- Flag references where no evidence of the publication can be found online
- Flag references where details don't match (e.g., wrong year, wrong journal, wrong authors)
- Check for retracted papers (if discoverable)

**Validation approach (4-tier):**

**Tier 0 — Zotero library (fastest, structured metadata):**
1. Read and search the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) for the reference by author `family` name and `issued` year
2. If found, compare the document's reference entry against the CSL-JSON metadata field by field (authors, title, journal, volume, issue, pages, DOI)
3. Any discrepancies between the document and Zotero = the document is wrong (Zotero imports from publisher databases)
4. Rate as **Verified (Zotero)** — highest confidence

**Tier 1 — Local PDFs:**
5. Search the local PDFs directory configured in `.local.md` for PDFs matching the reference: `Glob **/*AuthorSurname*.pdf` and `Glob **/*TitleKeyword*.pdf`
6. If found, read the PDF's first pages to extract full citation metadata
7. Cross-check against the reference entry

**Tier 2 — NotebookLM (if available):**
8. Query NotebookLM for the reference author/title to check if the researcher has collected it

**Tier 3 — Web search (fallback):**
9. For each reference NOT found in Zotero, local PDFs, or NotebookLM, search: `"Author Surname" "Article Title" "Year"`
10. Cross-check the returned results against the reference details
11. If DOI present, search: `doi:10.XXXX/YYYY` to confirm it resolves

**Rate confidence:** Verified (Zotero) / Verified (Local) / Verified (Web) / Likely Valid / Uncertain / Likely Fabricated / Cannot Verify

**Output fields per reference:**
| Field | Description |
|-------|-------------|
| Reference | The full reference as written |
| DOI Status | Valid / Invalid / Missing / Malformed |
| Publication Status | Verified / Likely Valid / Uncertain / Likely Fabricated |
| Details Match | Which fields match/don't match the real publication |
| Notes | Any discrepancies found |

### Agent 4: Claim Grounding Checker

**Purpose:** Verify that claims in the text are actually supported by the cited source. This catches misattributions and misrepresentations.

**Checks:**
- For each citation in context, extract the claim being made
- Determine if the cited source actually supports that claim
- Flag claims that seem inconsistent with the cited source's topic or field
- Flag statistical claims or specific findings attributed to sources that likely don't contain that data
- Flag over-generalisations (e.g., citing a single case study for a universal claim)
- Check that direct quotes are plausibly from the cited source

**Verification approach (4-tier):**

**Tier 0 — Zotero abstract (quick grounding check):**
1. Search the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) for the cited source by author and year
2. If found and `abstract` field is present, check the claim against the abstract text
3. Abstracts summarise key findings — enough to confirm or question many claims without reading the full paper

**Tier 1 — Local PDFs (full-text verification, highest confidence):**
4. Search the local PDFs directory configured in `.local.md` for the cited source's PDF using Glob (author surname, title keywords)
5. If found, READ the actual PDF (use Read tool with `pages` parameter for large files) and verify the claim against the source text
6. This upgrades assessments from "Plausible" to definitively "Grounded" or "Unsupported"
7. Note the local path in the output for the user's reference

**Tier 2 — NotebookLM (if available):**
8. Query the researcher's NotebookLM notebooks for the cited source to access annotated content

**Tier 3 — Title-based assessment (fallback):**
9. If the source is not available through any local channel, assess based on title, journal, and known content

**Confidence levels:**
- **Grounded** — claim clearly matches what the source covers
- **Plausible** — claim is in the source's domain but specifics can't be confirmed
- **Questionable** — claim seems inconsistent with the source's topic
- **Unsupported** — claim contradicts or has no connection to the cited source
- **Unverifiable** — insufficient information to assess

**Output fields per flagged claim:**
| Field | Description |
|-------|-------------|
| Location | Where in the document |
| Claim | The statement being made |
| Cited Source | The reference being used to support it |
| Assessment | Grounded / Plausible / Questionable / Unsupported |
| Reason | Why the assessment was made |
| Suggestion | Possible correction or alternative source |

### Agent 5: Completeness & Coverage Checker

**Purpose:** Find statements that should have citations but don't, and assess overall citation coverage.

**Checks:**
- Identify factual claims, statistics, definitions, and theoretical statements lacking citations
- Flag uncited claims in sections where citation density should be high (literature review, results)
- Allow lower citation density in introduction hooks and conclusion synthesis
- Identify sections that rely too heavily on a single source
- Flag long stretches of text (>2 paragraphs) without any citations
- Check for self-plagiarism signals (statements of fact presented as common knowledge that are actually specific claims)

**What does NOT need a citation (don't flag these):**
- Common knowledge ("Climate change is a global challenge")
- The author's own analysis and interpretation
- Methodological descriptions of the author's own study
- Logical connectors and transitions

**Output fields per issue:**
| Field | Description |
|-------|-------------|
| Location | Paragraph/section |
| Statement | The uncited claim |
| Issue | Missing citation / Over-reliance on single source / Citation desert |
| Severity | Error / Warning |
| Suggestion | What type of source would be needed |
| Zotero matches | Relevant references found in the Zotero CSL-JSON library with `[@citekey]` (if any) |
| Local sources | Relevant papers found in the local PDFs directory configured in `.local.md` (if any) |

**Source suggestions follow the 4-tier lookup:** First search the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) for relevant sources by topic keywords, title, or abstract — these come with ready-to-use `[@citekey]` citation keys. Then search the local PDFs directory configured in `.local.md` for local PDFs. Then check NotebookLM, then web search. Label suggestions with `[ZOTERO]`, `[LOCAL]`, or `[WEB]`. Prioritise sources the researcher already has.

## Execution Process

### Step 1: Parse the Document

Read the input document and extract:
1. **All in-text citations** — both parenthetical and narrative forms
2. **The reference list** — each entry as a structured record
3. **The full text** — segmented by section/paragraph for location tracking
4. **Document metadata** — title, word count, section headings

Present a summary to the user:
> Found **X in-text citations** across **Y pages/sections**, with **Z entries** in the reference list. Deploying 5 verification agents...

### Step 2: Deploy Agents

Launch all 5 agents in parallel using the Agent tool. Each agent receives:
- The extracted citations
- The reference list
- The full text (or relevant portions)
- Any source PDFs/articles provided by the user

### Step 3: Compile Report

Merge all agent results into a structured verification report.

## Verification Report Format

The final output is a markdown report with this structure:

```markdown
# Citation Verification Report

## Summary
- **Document:** [title/filename]
- **Total in-text citations:** X
- **Total references:** Y
- **Issues found:** Z (N errors, M warnings, P style notes)
- **Overall health:** ██████████░░ 82% (or similar visual indicator)

## Critical Issues (must fix)
[Sorted by severity — errors first]

### Fabricated/Unverifiable References
| # | Reference | Status | Details |
|---|-----------|--------|---------|

### Ghost Citations (cited but not in reference list)
| # | Citation | Location | Suggested Fix |
|---|----------|----------|---------------|

### Unsupported Claims
| # | Claim | Cited Source | Issue |
|---|-------|-------------|-------|

## Warnings (should fix)
### APA 7 Formatting Errors
...
### Orphan References (in reference list but never cited)
...
### Questionable Attributions
...

## Style Notes (optional fixes)
### Missing Citations
...
### Over-reliance on Single Sources
...

## Reference-by-Reference Status
| # | Reference | Cross-Ref | Format | DOI | Grounding | Overall |
|---|-----------|-----------|--------|-----|-----------|---------|
| 1 | Author (Year) | ✓ | ✓ | ✓ | ✓ | ✓ Pass |
| 2 | Author (Year) | ✓ | ✗ | — | ? | ⚠ Check |
...
```

### Step 4: Offer Fixes

After presenting the report, offer to run the companion skill `/scholar:citation-fixer` to apply corrections automatically. The fixer will:
1. **Auto-fix formatting issues** — correct APA 7 errors directly in the document
2. **Correct reference metadata** — fix wrong authors, volumes, DOIs via web search
3. **Insert `[CITATION NEEDED]` markers** — at uncited claims, with suggested sources
4. **Flag items for human review** — fabricated references, claim grounding issues, orphan references

The user can also choose to fix issues manually using the checker report as a guide.

## Reference Files

- `../../references/apa7-guide.md` — APA 7 formatting rules (symlink to academic-essay skill)
- `references/verification-patterns.md` — Common citation error patterns and how to detect them
