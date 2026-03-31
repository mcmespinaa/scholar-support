---
name: citation-fixer
description: "**Citation Fixer & Correction Agent**: Companion to citation-checker. Deploys 3 parallel agents to correct reference metadata, fix APA 7 formatting, and insert [CITATION NEEDED] markers. Creates backup before editing. Produces a changelog."
argument-hint: '"file-path" [scope: all|critical|formatting|gaps]'
user-invokable: true
---

# Citation Fixer & Correction Agent

## Overview

This skill is the companion to `citation-checker`. Where the checker audits and reports, the fixer acts. It deploys 3 parallel agents to correct reference metadata, fix APA 7 formatting, and mark citation gaps — then applies all changes to the document and produces a changelog. It creates a backup before editing and never auto-fixes issues that require human judgment.

## When to Use This Skill

Use when the user asks you to:
- Fix or correct citations after running citation-checker
- Apply corrections from a verification report
- Clean up APA 7 formatting errors in citations and references
- Add missing DOIs to a reference list
- Fix wrong author names, volumes, or page numbers in references
- Insert citation-needed markers in a draft
- Repair formatting artifacts (stray backslashes, wrong punctuation)

## Input Requirements

**Required (one of):**
1. **Document + citation-checker report** — the document to fix and the checker's output (from conversation history or a file)
2. **Document only** — the fixer will run `/scholar:citation-checker` first, then proceed to fixes

**Optional:**
- **Fix scope** — "all", "critical only", "formatting only", or "gaps only"
- **Target format** — defaults to APA 7

## Source Lookup Priority

Agents that search for correct metadata or suggest sources follow a **4-tier lookup chain**, checking the researcher's own sources before web searching:

```
0. ZOTERO: Search the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) by author, title, year
   ├── Match found → Use structured metadata as ground truth for corrections
   └── No match ↓
1. LOCAL PDFs: Search the local PDFs directory configured in `.local.md` by author surname, title keywords, or topic
   ├── Match found → Read PDF to extract correct metadata or verify content
   └── No match ↓
2. NOTEBOOKLM: Query researcher's NotebookLM notebooks (if available)
   ├── Match found → Use notebook content
   └── No match or not configured ↓
3. WEB SEARCH: Search via WebSearch/WebFetch (fallback)
```

**Why this order:**
- **Zotero first:** CSL-JSON has structured metadata imported from publisher databases — ideal for correcting authors, DOI, volume, pages. Also provides `[@citekey]` for gap marker suggestions.
- **Local PDFs second:** For content-level verification or when the reference isn't in Zotero yet.
- **NotebookLM third:** Researcher's annotated notebooks.
- **Web search last:** Only for sources not available locally.
- Label suggestions with `[ZOTERO]`, `[LOCAL]`, or `[WEB]` so the researcher can prioritise.

## Safety: What Gets Fixed vs. What Gets Flagged

This is the most important section. The fixer draws a hard line between safe auto-fixes and issues that require human judgment.

### Auto-Fixed (applied directly to document)

| Category | Examples |
|----------|----------|
| Formatting artifacts | Stray backslashes (`2007\)` → `2007)`), escaped periods |
| DOI format | `doi:10.1234/abc` → `https://doi.org/10.1234/abc` |
| Missing DOIs | Web-searched and appended to references that have them |
| Wrong volume/issue/pages | Corrected via web search against real publication metadata |
| Wrong author lists | Corrected via web search (only when real publication is confirmed) |
| Ampersand/and usage | `&` in parenthetical, `and` in narrative citations |
| et al. usage | Corrected for 3+ authors from first citation |
| Capitalisation | Sentence case for article titles, title case for journal names |
| Edition format | `Second Edition` → `2nd ed.` |
| "Retrieved from" removal | Deleted per APA 7 |
| Punctuation placement | Period after parenthetical citation, not before |
| Reference list ordering | Re-sorted alphabetically by first author surname |
| Incomplete references | Missing fields filled via web search (journal, volume, pages, URL) |

### Flagged for Human Review (NOT auto-fixed)

| Category | Why |
|----------|-----|
| Fabricated/likely-fabricated references | User must decide: find real source, replace, or delete |
| Claim grounding issues | Misattributed claims require reading the source |
| Orphan references | User may want to add a citation rather than delete the reference |
| Unsupported/questionable attributions | Requires content judgment |
| Over-reliance on single source | Structural decision about the argument |
| Citation deserts | User must decide what to cite |

## Agent Architecture

```
                 ┌──────────────────────────┐
                 │      COORDINATOR         │
                 │  (parse report, backup,  │
                 │   merge, apply fixes)    │
                 └───────────┬──────────────┘
                             │ launches 3 agents in parallel
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │ Agent A  │  │ Agent B  │  │ Agent C  │
        │ Metadata │  │ Format   │  │ Gap      │
        │ Corrector│  │ Fixer    │  │ Marker   │
        └──────────┘  └──────────┘  └──────────┘
```

### Agent A: Metadata Corrector

**Purpose:** Fix incorrect publication metadata in references by searching for the real publication details.

**Input:** References flagged with wrong authors, wrong volumes/issues/pages, missing DOIs, incomplete entries, or "Uncertain" verification status.

**Process (4-tier lookup):**

**Tier 0 — Zotero library (fastest, most reliable for metadata):**
1. Read and search the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) for the flagged reference by author `family` name and `issued` year
2. If found, use the CSL-JSON fields directly as the correct metadata (authors, title, journal, volume, issue, pages, DOI)
3. Zotero imports metadata from publisher databases — this is authoritative
4. Apply corrections immediately: replace wrong values in the document with Zotero's values

**Tier 1 — Local PDFs:**
5. Search the local PDFs directory configured in `.local.md` for the flagged reference's PDF using Glob: `**/*AuthorSurname*.pdf`, `**/*TitleKeyword*.pdf`
6. If found, read the PDF's first pages to extract correct metadata

**Tier 2 — NotebookLM (if available):**
7. Query the researcher's NotebookLM notebooks for the reference

**Tier 3 — Web search (fallback):**
8. For references NOT found in Zotero, local PDFs, or NotebookLM, web-search: `"First Author Surname" "Article Title" "Year"`
9. Cross-check returned results against the reference
10. If DOI is missing, search: `"Article Title" doi` or check the publisher's website
11. For wrong author lists: confirm the correct authors from the publisher page
12. For wrong volume/issue/pages: extract correct metadata from the publisher page or database record

**Does NOT fix:**
- References marked "Likely Fabricated" — instead reports the closest real match found and flags for human review
- References where web search returns conflicting information — flags as uncertain

**Output per fix:**

| Field | Description |
|-------|-------------|
| Reference # | Which reference in the list |
| Field | What was corrected (authors, volume, DOI, etc.) |
| Before | Original value |
| After | Corrected value |
| Source | URL where correct metadata was found |
| Confidence | High / Medium (no Low — if Low, flag for human review instead) |

### Agent B: Format Fixer

**Purpose:** Correct all APA 7 formatting violations in both in-text citations and the reference list.

**Input:** All formatting errors from the checker report.

**Fix categories (in priority order):**

1. **Stray characters** — remove backslashes, escaped characters, formatting artifacts
2. **DOI format** — convert `doi:`, `dx.doi.org`, or bare DOIs to `https://doi.org/` format
3. **Ampersand/and** — `&` inside parenthetical citations, `and` in narrative citations
4. **et al.** — use from first citation for 3+ authors; never for 2 authors
5. **Capitalisation** — sentence case for article/chapter titles; title case for journal names and book titles
6. **Italicisation** — add markdown italic markers (`*...*`) for journal names, volumes, and book titles in `.md` files
7. **Edition format** — `(2nd ed.)` not `(Second Edition)` or `(2nd edition)`
8. **"Retrieved from"** — remove (APA 6 convention, not APA 7)
9. **Punctuation** — period after closing parenthesis of parenthetical citations
10. **Author name format** — `Surname, A. A.` with `&` before last author
11. **Page numbers** — `p.` for single page, `pp.` for range
12. **Issue numbers** — in parentheses after volume, not italicised

**Reference:** Read `../../references/apa7-guide.md` for the complete formatting rules.

**Output per fix:**

| Field | Description |
|-------|-------------|
| Location | Line number or reference # |
| Type | In-text citation or reference list |
| Before | Current text |
| After | Corrected text |
| Rule | Which APA 7 rule was applied |

### Agent C: Gap Marker

**Purpose:** Insert visible markers at uncited claims and suggest potential sources. Does NOT insert actual citations — that requires human judgment about which source to use.

**Input:** Missing citation flags, citation desert warnings, and over-reliance warnings from the checker report.

**Markers inserted:**

| Situation | Marker |
|-----------|--------|
| Factual claim without citation | `[CITATION NEEDED]` |
| Citation desert (>2 paragraphs without citations) | `[CITATION DESERT — consider adding sources]` |
| Over-reliance on single source | `[DIVERSIFY SOURCES]` |

**Source suggestion process (4-tier lookup):**

**Tier 0 — Zotero library (best — provides ready-to-use citation keys):**
1. For each uncited claim, extract key terms from the statement
2. Search the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) by title, abstract, and keyword fields for relevant sources
3. If found, label as `[ZOTERO]` and include the `[@citekey]` — the researcher can paste it directly

**Tier 1 — Local PDFs:**
4. Search the local PDFs directory configured in `.local.md` for relevant local papers by topic: `Glob **/*keyword*.pdf`
5. If found, label as `[LOCAL]`

**Tier 2 — NotebookLM (if available):**
6. Query NotebookLM for sources related to the uncited claim

**Tier 3 — Web search (fallback):**
7. Web-search for relevant academic sources: `"key terms" site:scholar.google.com` or `"key terms" journal article`
8. Label as `[WEB]`

9. Suggest 1-3 potential sources with author, year, title, and URL/path/citekey
10. Do NOT insert these as actual citations — list them in the changelog as suggestions

**Suggestion format in HTML comments:**
```markdown
<!-- Suggested sources:
- [ZOTERO] Author (Year). Title. Use: [@citekey]
- [LOCAL] Author (Year). Title. Found at: the local PDFs directory configured in `.local.md`/Course/file.pdf
- [WEB] Author (Year). Title. https://doi.org/...
-->
```

**Output per marker:**

| Field | Description |
|-------|-------------|
| Location | Line number or section/paragraph |
| Statement | The uncited claim |
| Marker | Which marker was inserted |
| Suggested sources | 1-3 potential references with URLs |

## Execution Process

### Step 1: Accept Input

Determine the entry point:
- **Document + checker report in conversation:** Parse the report from conversation history
- **Document + report file:** Read the report file
- **Document only:** Run `/scholar:citation-checker` first, then proceed

Present a summary:
> Found **X issues** in the checker report: **Y auto-fixable**, **Z for human review**. Creating backup and deploying 3 correction agents...

### Step 2: Create Backup

Before any edits, copy the original file:
```
<filename>_backup_YYYYMMDD_HHMMSS.<ext>
```

Confirm to the user:
> Backup saved: `Espina_Research_Proposal_OL652E_backup_20260312_143022.md`

### Step 3: Categorise Issues

Parse the checker report and sort each issue:
- **Auto-fixable** → assign to Agent A, B, or C
- **Human review** → collect for the final report

### Step 4: Deploy Agents

Launch all 3 agents in parallel using the Agent tool. Each agent receives:
- The document content
- Their assigned subset of issues from the checker report
- Reference files as needed (Agent B gets `apa7-guide.md`)

### Step 5: Merge Results

Collect all proposed fixes from the 3 agents. Check for conflicts:
- If two agents propose changes to the same text, apply in priority order: **Metadata (A) > Format (B) > Gap markers (C)**
- Metadata changes alter content; format changes alter presentation; gap markers add new text

### Step 6: Apply Fixes

Apply all changes to the document using the Edit tool. **Apply bottom-to-top** (highest line numbers first) to preserve line number accuracy for subsequent edits.

### Step 7: Output Report

Produce the changelog and human review report (see Output Format below).

## Output Format

```markdown
# Citation Fixer — Changelog

## Summary
- **Document:** [filename]
- **Backup:** [backup filename]
- **Fixes applied:** X total (Y metadata, Z formatting, W gap markers)
- **Items for human review:** N

## Applied Fixes

### Metadata Corrections (Agent A)
| # | Reference | Field | Before | After | Source |
|---|-----------|-------|--------|-------|--------|

### Formatting Fixes (Agent B)
| # | Location | Type | Before | After | Rule |
|---|----------|------|--------|-------|------|

### Gap Markers Inserted (Agent C)
| # | Location | Statement | Marker | Suggested Sources |
|---|----------|-----------|--------|-------------------|

## Requires Human Review

### Potentially Fabricated References
| # | Reference | Checker Status | Closest Real Match | Action Needed |
|---|-----------|---------------|-------------------|---------------|

### Claim Grounding Issues
| # | Location | Claim | Cited Source | Issue |
|---|----------|-------|-------------|-------|

### Orphan References
| # | Reference | Options |
|---|-----------|---------|

### Citation Coverage Issues
| # | Location | Issue | Suggestion |
|---|----------|-------|------------|
```

## File Format Handling

| Format | Approach |
|--------|----------|
| `.md` / `.txt` | Full auto-fix: text edits, markdown italics, markers |
| `.docx` | Use python-docx or pandoc for text edits; flag italicisation for manual review |
| `.pdf` | Cannot edit; output corrections as a standalone report only |

## Reference Files

- `../../references/apa7-guide.md` — APA 7 formatting rules for citations and reference lists
- `references/correction-rules.md` — Auto-fix transformation patterns and safety boundaries
