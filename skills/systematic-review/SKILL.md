---
name: systematic-review
description: "**Systematic Literature Review Pipeline**: End-to-end systematic review — from research question through search strategy, screening, data extraction, quality assessment, synthesis, and PRISMA 2020-compliant manuscript. Produces .docx manuscript and .xlsx review matrix."
argument-hint: '"research topic or question" [phase: 1-7] [framework: PICO|PCC]'
user-invokable: true
---

# Systematic Literature Review Pipeline

## Overview

This skill runs a full systematic literature review from question to manuscript. It produces two deliverables: a PRISMA 2020-compliant `.docx` manuscript and a `.xlsx` review matrix with extracted data. The skill is optimized for social sciences research but adapts to any discipline.

The pipeline has seven phases. The user can enter at any phase depending on how far along they are — some users arrive with just a topic, others already have a stack of articles. The skill detects the entry point and picks up from there.

## Phase Detection

Before starting, figure out where the user is:

| User says something like... | Start at |
|----------------------------|----------|
| "I want to review literature on X" | Phase 1 |
| "Here's my research question, help me search" | Phase 2 |
| "I have these articles, help me screen them" | Phase 3 |
| "I need to extract data from these studies" | Phase 4 |
| "Help me assess quality of these studies" | Phase 5 |
| "I have my data, help me synthesize" | Phase 6 |
| "Write up my systematic review" | Phase 7 |

If the user provides articles or references alongside their request, acknowledge them and integrate them into the appropriate phase rather than starting from scratch.

---

## Phase 1: Research Question Formulation

The research question shapes everything that follows. A weak question produces an unfocused review.

### PICO Framework (for intervention-focused reviews)

| Element | Question to ask | Example |
|---------|----------------|---------|
| **P** Population | Who is being studied? | Smallholder farmers in sub-Saharan Africa |
| **I** Intervention | What is being examined? | Digital agricultural extension services |
| **C** Comparison | Compared to what? | Traditional in-person extension |
| **O** Outcome | What is measured? | Crop yield, adoption rates, income |

### PCC Framework (for scoping or exploratory reviews)

| Element | Question to ask | Example |
|---------|----------------|---------|
| **P** Population | Who? | Social entrepreneurs |
| **C** Concept | What concept? | Impact measurement practices |
| **C** Context | Where/when? | European third-sector organisations, 2015-present |

### Deliverable

A one-paragraph research question statement formatted as:

> **Research Question:** In [Population], how does [Intervention] compared to [Comparison] affect [Outcome]?

Plus 2-3 sub-questions if the review scope warrants them.

---

## Phase 2: Search Strategy Design

This phase produces a reproducible, documented search strategy. Read `references/search-strategy-templates.md` for database-specific syntax and Boolean patterns.

### Step 2.1: Identify search terms

Break the research question into concept groups (typically one per PICO element). For each group, generate:

- Primary terms (from the research question)
- Synonyms and related terms
- Controlled vocabulary / subject headings (where applicable)
- Truncated forms (e.g., `sustainab*`, `entrepren*`)

Present as a **concept table**:

| Concept | Terms |
|---------|-------|
| Population | "smallholder farmer*" OR "small-scale farmer*" OR "subsistence farmer*" |
| Intervention | "digital extension" OR "mobile advisory" OR "e-extension" OR "ICT-based extension" |
| Outcome | "crop yield" OR "agricultural productivity" OR "farm income" OR "technology adoption" |

### Step 2.2: Build search strings

Combine concept groups with AND:

```
("smallholder farmer*" OR "small-scale farmer*" OR "subsistence farmer*")
AND
("digital extension" OR "mobile advisory" OR "e-extension" OR "ICT-based extension")
AND
("crop yield" OR "agricultural productivity" OR "farm income" OR "technology adoption")
```

### Step 2.3: Select databases

For social sciences, default to:

| Database | Strength | Access |
|----------|----------|--------|
| Scopus | Broad multidisciplinary coverage, good for social sciences | Institutional |
| Web of Science | High-quality journals, strong citation analysis | Institutional |
| ERIC | Education research | Free |
| PubMed/MEDLINE | Health and biomedical | Free |
| Google Scholar | Supplementary grey literature search | Free |

Adapt based on the user's discipline and institutional access.

### Step 2.4: Define inclusion/exclusion criteria

Use 10 categories. Present as a table:

| Category | Inclusion | Exclusion |
|----------|-----------|-----------|
| Date | 2015-2025 | Before 2015 |
| Language | English | Non-English |
| Publication type | Peer-reviewed journal articles | Conference abstracts, book reviews |
| Study design | Empirical (qualitative, quantitative, mixed) | Theoretical/conceptual only |
| Population | [from PICO] | [not matching PICO] |
| Intervention/Exposure | [from PICO] | [not matching PICO] |
| Outcome | Reports at least one relevant outcome | No relevant outcomes |
| Geographic scope | Any (or specify) | — |
| Setting | [specify if relevant] | — |
| Peer review | Peer-reviewed | Non-peer-reviewed (unless grey lit included) |

### Deliverable

A **Search Protocol Document** containing: research question, concept table, full Boolean search strings for each database, inclusion/exclusion criteria table, and a search log template.

---

## Phase 3: Study Selection & Screening

### Step 3.1: Simulate PRISMA flow

Since we cannot run actual database searches, work with what the user provides:

- If user provides a list of articles: use those as the starting pool
- If user provides search results (exported references): work from those
- If user provides nothing: help them design the search (Phase 2) and ask them to run it

### Step 3.2: Two-stage screening

**Stage 1 — Title & Abstract:**
- Apply inclusion/exclusion criteria
- When uncertain, include for full-text review
- Document reason for each exclusion

**Stage 2 — Full-text:**
- Apply detailed eligibility criteria
- Document specific exclusion reasons (these go in the PRISMA flow diagram)

### Step 3.3: Document PRISMA flow numbers

Track and record:
- Records identified from databases (per database)
- Records from other sources
- Duplicates removed
- Records screened (title/abstract)
- Records excluded at screening
- Full-text articles assessed
- Full-text articles excluded (with reasons)
- Studies included in final synthesis

### Deliverable

A populated PRISMA flow diagram (as a formatted table in the manuscript) and a screening log.

---

## Phase 4: Data Extraction

### Step 4.1: Design extraction form

Create a review matrix with these standard columns (adapt based on the review topic):

| Column | Description |
|--------|-------------|
| ID | Sequential study identifier (S1, S2, ...) |
| Author(s) | First author et al. |
| Year | Publication year |
| Title | Full article title |
| Journal | Journal name |
| Country/Region | Study location |
| Study Design | RCT, cohort, cross-sectional, qualitative, mixed methods |
| Population | Sample characteristics (n, demographics) |
| Intervention/Exposure | What was studied |
| Comparison | Control or comparator (if any) |
| Outcomes Measured | Primary and secondary outcomes |
| Key Findings | Main results with effect sizes where applicable |
| Limitations | Study-level limitations noted by authors |
| Quality Score | Result of critical appraisal (Phase 5) |
| Notes | Reviewer notes, funding, conflicts of interest |

### Step 4.2: Populate the matrix

Extract data from each included study into the matrix. If the user provides article content (PDFs, summaries, notes), extract directly. If not, work from what's available (titles, abstracts, user descriptions).

### Deliverable

A `.xlsx` review matrix file. See "Review Matrix (.xlsx) Generation" below for technical details.

---

## Phase 5: Quality Assessment

### Select tool based on study designs

| Study Design | Assessment Tool | Reference |
|-------------|----------------|-----------|
| RCTs | Cochrane Risk of Bias (RoB 2) | Sterne et al. (2019) |
| Cohort studies | Newcastle-Ottawa Scale (NOS) | Wells et al. |
| Cross-sectional | JBI Checklist for Analytical Cross-Sectional | JBI (2020) |
| Qualitative | CASP Qualitative Checklist | CASP (2018) |
| Mixed methods | Mixed Methods Appraisal Tool (MMAT) | Hong et al. (2018) |

### Assessment process

For each included study:
1. Identify the study design
2. Apply the appropriate quality assessment tool
3. Rate each domain (Low risk / Some concerns / High risk, or equivalent)
4. Assign overall quality rating
5. Add the score to the review matrix

### Deliverable

Quality assessment results added to the review matrix, plus a summary table showing risk of bias across studies.

---

## Phase 6: Data Synthesis

### Narrative synthesis (default for social sciences)

Organize findings thematically rather than study-by-study. The synthesis should:

1. **Identify themes** that emerge across studies
2. **Compare and contrast** findings within each theme
3. **Note patterns** — where do studies agree? Where do they diverge?
4. **Explain heterogeneity** — why might studies find different things?
5. **Map gaps** — what hasn't been studied?

### Synthesis structure pattern

For each theme:
- State the theme and how many studies address it
- Present the consensus findings (with citations)
- Present the divergent findings (with citations)
- Discuss possible explanations for differences
- Connect to the research question

### Quantitative synthesis (if applicable)

If studies are sufficiently homogeneous and report comparable effect measures, consider:
- Pooled effect estimates
- Forest plots
- Heterogeneity statistics (I², Cochran's Q)
- Sensitivity analyses

Most social sciences reviews use narrative synthesis. Only attempt meta-analysis if the user's studies are appropriate for it.

### Deliverable

A structured synthesis narrative organized by themes, ready for integration into the manuscript.

---

## Phase 7: Manuscript Production

This phase produces the final `.docx` manuscript. Read the docx skill patterns (the skill uses `docx-js` via Node.js) for technical implementation.

### Manuscript structure (PRISMA 2020)

| Section | Content | Approximate proportion |
|---------|---------|----------------------|
| Title Page | Title (include "systematic review"), author, affiliation, date | — |
| Abstract | Structured: Background, Methods, Results, Conclusions | ~250 words |
| 1. Introduction | Rationale, objectives, research question(s) | 10-15% |
| 2. Methods | Protocol, eligibility, sources, search strategy, selection, extraction, quality assessment, synthesis | 25-30% |
| 3. Results | Study selection (PRISMA flow), characteristics, quality, synthesis by theme | 35-40% |
| 4. Discussion | Interpretation, comparison with other reviews, limitations, implications | 15-20% |
| 5. Conclusion | Summary of evidence, recommendations, future research | 5% |
| References | APA 7 format | — |
| Appendices | Full search strings, excluded studies list, quality assessment details | — |

### Writing standards

**Academic voice:**
- Third person unless discipline conventions differ
- Hedging for uncertain claims ("suggests," "indicates," "appears to")
- Active voice for clarity where appropriate
- Define technical terms on first use

**Citation density:**
- Introduction: moderate
- Methods: low (citing methodology sources)
- Results: high (every finding attributed to source studies)
- Discussion: moderate (comparing with external literature)

**Citation format — APA 7:**
Read `../../references/apa7-guide.md` for the complete reference.

| Situation | Format |
|-----------|--------|
| 1 author | (Author, Year) |
| 2 authors | (Author & Author, Year) |
| 3+ authors | (Author et al., Year) |
| Direct quote | (Author et al., Year, p. XX) |
| Narrative | Author et al. (Year) found that... |

---

## Review Matrix (.xlsx) Generation

Use `openpyxl` to create the review matrix spreadsheet.

### Structure

**Sheet 1: "Review Matrix"** — the main data extraction table with all columns from Phase 4.

**Sheet 2: "Quality Assessment"** — risk of bias / quality scores per study per domain.

**Sheet 3: "PRISMA Flow"** — the flow diagram numbers in tabular form.

### Formatting

```python
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill, Alignment, Border, Side

# Header row: bold, dark background, white text
header_fill = PatternFill('solid', fgColor='1B4332')
header_font = Font(name='Arial', size=11, bold=True, color='FFFFFF')

# Data rows: alternating light fills for readability
alt_fill = PatternFill('solid', fgColor='F0F7F4')

# Column widths: set generously for readability
# ID: 8, Authors: 25, Year: 10, Title: 40, etc.
```

After creating, recalculate formulas if any are used:
```bash
python scripts/recalc.py review_matrix.xlsx
```

---

## Manuscript (.docx) Generation

Use `docx-js` (Node.js) to create the manuscript. Install: `npm install -g docx`

### Page setup
```javascript
// Portrait, US Letter, 1-inch margins
{ width: 12240, height: 15840, margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } }
```

### Typography
- Body: Arial, 12pt (size: 24 half-points)
- Line spacing: double (line: 480)
- Paragraph spacing: 200 after
- Headings: Bold, hierarchical (H1: 16pt, H2: 13pt, H3: 12pt)

### Reference list
- Hanging indent: `indent: { left: 720, hanging: 720 }`
- Alphabetical by first author surname
- Include DOI as hyperlink where available
- Font size: 11pt (size: 22)

### PRISMA flow diagram (table format)

Render the PRISMA flow as a structured table in the manuscript with four sections (Identification, Screening, Eligibility, Included), using cell shading and borders to create a clean visual flow.

### Tables in the manuscript

Include at minimum:
1. **Table 1:** Characteristics of included studies (summary of review matrix)
2. **Table 2:** Quality assessment summary
3. **PRISMA Flow Diagram** (as formatted table)

---

## Zotero Integration (Citation Helper)

Use the `/scholar:citation-helper` skill to search the researcher's Zotero library (the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`)) throughout the review process.

### Integration Points by Phase

| Phase | How Zotero Helps |
|-------|-----------------|
| **Phase 2: Search Strategy** | Search Zotero to see what the researcher already has on the topic — helps identify terms and scope |
| **Phase 3: Screening** | Cross-reference candidate articles against the Zotero library to check which ones the researcher already has |
| **Phase 4: Data Extraction** | Use CSL-JSON metadata for accurate author names, DOIs, journal names, volumes, and pages |
| **Phase 6: Synthesis** | Search Zotero for additional context sources to compare findings against |
| **Phase 7: Manuscript** | Use `[@citekey]` format for all citations; generate the reference list via Operation 4 |

### Citation Workflow During Manuscript Writing

1. **Insert Pandoc citation keys:** Use `[@citekey]` from the CSL-JSON `id` field
2. **Multiple sources:** `[@key1; @key2; @key3]`
3. **Narrative citations:** `@citekey` (e.g., `@moore.westley2011 found that...`)
4. **Generate reference list:** Use `/scholar:citation-helper` Operation 4 after writing is complete

### Source Lookup Priority

```
0. ZOTERO: Search the Zotero CSL-JSON library configured in the plugin's .local.md file (default: references/salsu-library.json) by author, title, keyword, year
   ├── Match found → Use [@citekey] — researcher already has this source
   └── No match ↓
1. LOCAL PDFs: Search the local PDFs directory configured in .local.md for relevant papers
   └── No match ↓
2. WEB SEARCH: Search for additional academic sources (fallback)
```

### Pandoc Rendering

To produce a formatted `.docx` with resolved APA 7 citations and automatic reference list:
```bash
# Replace <path-to-csl-json-library> and <path-to-apa-csl> with your configured paths from .local.md
pandoc manuscript.md \
  --citeproc \
  --bibliography=<path-to-csl-json-library> \
  --csl=<path-to-apa-csl> \
  -o manuscript.docx
```

## Process Summary

```
Phase 1: Research Question  →  PICO/PCC framework
         ↓
Phase 2: Search Strategy    →  Concept table, Boolean strings, database selection
         ↓                     + search Zotero library for existing sources
Phase 3: Screening          →  Title/abstract + full-text, PRISMA flow numbers
         ↓
Phase 4: Data Extraction    →  Review matrix (.xlsx), use Zotero metadata
         ↓
Phase 5: Quality Assessment →  Risk of bias per study, added to matrix
         ↓
Phase 6: Synthesis          →  Thematic narrative synthesis with [@citekey] citations
         ↓
Phase 7: Manuscript         →  PRISMA-compliant .docx (Pandoc --citeproc for citations)
```

## Reference Files

- `../../references/apa7-guide.md` — Complete APA 7 formatting rules for citations and reference lists
- `../../references/prisma-checklist.md` — PRISMA 2020 reporting items checklist with flow diagram template
- `references/search-strategy-templates.md` — Database-specific search syntax (Scopus, WoS, PubMed, ERIC) and Boolean patterns
- `../../references/review-methodology.md` — Detailed methodology guide covering all seven phases with examples and common pitfalls
