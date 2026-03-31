---
name: academic-essay
description: "**Academic Essay & Literature Review Writer**: Creates structured academic essays, systematic literature reviews, scoping reviews, and research papers with PRISMA compliance, PICO frameworks, and APA 7 citations. Outputs professional .docx files."
argument-hint: '"topic or assignment brief" [type: essay|systematic-review|scoping-review] [word-count]'
user-invokable: true
---

# Academic Essay & Systematic Literature Review Skill

## Overview

This skill produces professional academic essays and literature reviews as `.docx` files. It covers the full spectrum from standard argumentative essays to PRISMA-compliant systematic and scoping reviews. The skill embeds best practices from established review methodologies (PRISMA 2020, PRISMA-ScR, PROSPERO, JBI Manual) while remaining flexible for simpler essay formats.

## When to Use This Skill

Use when the user asks you to:
- Write an academic essay on any topic
- Conduct or write up a systematic literature review
- Produce a scoping review following PRISMA-ScR guidelines
- Create a research synthesis or critical review paper
- Write a course essay requiring theoretical argumentation and citations
- Draft a manuscript following systematic review templates
- Help with any academic writing that requires evidence-based argumentation

## Document Types & Structures

### Type 1: Standard Academic Essay

For course essays, argumentative papers, critical analyses, and reflection papers.

**Structure:**
1. **Title Page** (title, author, course, institution, date)
2. **Introduction** (~10-15% of total words)
   - Hook/context
   - Problem statement or research question
   - Thesis statement
   - Essay roadmap
3. **Body Sections** (~70-80% of total words)
   - Each section = one major argument or theme
   - Topic sentence, evidence, analysis, connection to thesis
   - Theory integration throughout
4. **Conclusion** (~10-15% of total words)
   - Restate thesis (rephrased)
   - Synthesise key arguments
   - Implications or future directions
5. **References** (APA 7)

### Type 2: Systematic Literature Review

For PRISMA 2020-compliant systematic reviews of empirical research.

**Structure (PRISMA 2020):**
1. **Title** (identify as systematic review; include "systematic review" in title)
2. **Structured Abstract** (Background, Methods, Results, Discussion, Conclusions)
3. **Introduction** (rationale, objectives, research questions)
4. **Methods**
   - Registration and protocol (PROSPERO ID if applicable)
   - Eligibility criteria (PICO framework)
   - Information sources (databases, date ranges)
   - Search strategy (full strategy for at least one database)
   - Selection process (screening steps, reviewer count)
   - Data collection process
   - Data items
   - Risk of bias assessment
   - Effect measures
   - Synthesis methods
5. **Results**
   - Study selection (PRISMA flow diagram)
   - Study characteristics
   - Risk of bias findings
   - Results of individual studies
   - Results of syntheses
6. **Discussion** (interpretation, limitations, implications)
7. **References**
8. **Appendices** (search strategies, excluded studies, quality assessment tools)

### Type 3: Scoping Review

For PRISMA-ScR compliant scoping reviews that map the extent of research on a topic.

**Key differences from systematic review:**
- Broader research questions (exploratory rather than evaluative)
- Uses "Charting Methods" instead of "Synthesis Methods"
- Critical appraisal is optional (not mandatory)
- Focus on mapping the landscape rather than synthesising effect sizes
- Results presented as thematic categories or conceptual maps

**Structure follows PRISMA-ScR with adaptations from JBI Manual and Arksey & O'Malley (2005) 6-step framework:**
1. Identify research question(s)
2. Create search strategy (PCC: Population-Concept-Context)
3. Identify relevant studies
4. Review and select studies
5. Extract and chart data
6. Analyse and present results

## Systematic Review Methodology

### PICO Framework (Question Formulation)

| Element | Description | Example |
|---------|-------------|---------|
| **P** (Population) | Who are the participants? | Adults aged 18-65 with type 2 diabetes |
| **I** (Intervention) | What is being studied? | Digital health interventions |
| **C** (Comparison) | Against what? | Standard care or no intervention |
| **O** (Outcome) | What is measured? | HbA1c levels, quality of life |

**Five question types:**
- **Therapy:** Does intervention X improve outcome Y in population Z?
- **Etiology:** Does exposure X increase risk of outcome Y?
- **Diagnosis:** How accurate is test X for condition Y?
- **Prevention:** Does intervention X reduce incidence of Y?
- **Prognosis:** What happens to patients with condition X over time?

### Inclusion/Exclusion Criteria (10 Categories)

Define clear criteria across these dimensions:
1. Date restrictions (publication year range)
2. Exposure/intervention of interest
3. Geographic location
4. Language
5. Participants/population
6. Peer review status
7. Reported outcomes
8. Setting
9. Study design
10. Type of publication

### Search Strategy

**Database selection:** Choose databases appropriate to the field:
- Health: PubMed/MEDLINE, CINAHL, Cochrane, Embase, PsycINFO
- Social Sciences: Scopus, Web of Science, ERIC, ProQuest
- Multidisciplinary: Google Scholar (supplementary only)

**Search documentation:** Record for each database:
- Database name and platform
- Date of search
- Complete search string (Boolean operators, MeSH terms, wildcards)
- Number of results
- Export format

### Review Matrix

For organising extracted data from included studies:

| Author(s) | Year | Population | Intervention/Exposure | Outcomes | Quality Appraisal | Key Findings |
|-----------|------|------------|----------------------|----------|-------------------|--------------|

### PRISMA Flow Diagram

Document study selection across four phases:
- **Identification:** Records from databases + other sources
- **Screening:** Records after duplicates removed, records screened, records excluded
- **Eligibility:** Full-text articles assessed, articles excluded with reasons
- **Included:** Studies in qualitative synthesis, studies in quantitative synthesis (if meta-analysis)

## Writing Quality Standards

### Academic Voice
- Third person unless discipline conventions allow first person
- Hedging language for uncertain claims ("suggests," "indicates," "may")
- Active voice for clarity where appropriate
- Avoid colloquialisms, contractions, and informal language
- Define technical terms on first use

### Argumentation Structure
Each body paragraph should follow this pattern:
1. **Topic sentence** (claim)
2. **Evidence** (citation + data)
3. **Analysis** (interpret the evidence)
4. **Connection** (link back to thesis/research question)

### Critical Engagement with Sources
- Do not merely summarise sources; analyse, compare, and synthesise
- Identify agreements, contradictions, and gaps across studies
- Evaluate methodological quality and limitations
- Position your argument within the existing debate

### Citation Density
- Introduction: moderate (establish context)
- Literature review/body: high (every claim needs support)
- Discussion: moderate (interpretation + new citations)
- Conclusion: low (synthesis of own arguments)

### Citation Style (APA 7)

Read `../../references/apa7-guide.md` for the complete APA 7 reference.

| Situation | Format |
|-----------|--------|
| 1 author | (Author, Year) |
| 2 authors | (Author & Author, Year) |
| 3+ authors | (Author et al., Year) |
| Direct quote | (Author et al., Year, p. XX) |
| Narrative | Author et al. (Year) argue that... |

## Document Generation

Use the `docx` skill's infrastructure for Word document creation.

### Page Setup
```javascript
// Standard portrait
{ width: 12240, height: 15840, margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } }
```

### Typography
- Body: Arial or Times New Roman, 12pt (size: 24 half-points)
- Line spacing: double (line: 480)
- Paragraph spacing: 200 after
- Headings: Bold, hierarchical sizing (H1: 16pt, H2: 13pt, H3: 12pt)

### Reference List Formatting
- Hanging indent: `indent: { left: 720, hanging: 720 }`
- Alphabetical by first author surname
- Include DOI as hyperlink where available

## Zotero Integration (Citation Helper)

When drafting content that requires citations, use the `/scholar:citation-helper` skill to search the researcher's Zotero library (the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`)) before web searching for sources.

### Citation Workflow During Drafting

1. **For each claim needing a citation:** Search the Zotero library first using `/scholar:citation-helper` (Operation 1: Search Library or Operation 5: Suggest Citations)
2. **Insert Pandoc citation keys:** Use `[@citekey]` format from the CSL-JSON `id` field (e.g., `[@moore.westley2011]`)
3. **Multiple sources:** `[@key1; @key2; @key3]`
4. **Narrative citations:** `@citekey` (e.g., `@moore.westley2011 argue that...`)
5. **With page numbers:** `[@citekey, p. 15]`

### Source Lookup Priority

When searching for sources to cite, follow this order:

```
0. ZOTERO: Search the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) by author, title, keyword, year
   ├── Match found → Use [@citekey] directly — researcher already has this source
   └── No match ↓
1. LOCAL PDFs: Search the local PDFs directory configured in `.local.md` for relevant papers
   └── No match ↓
2. WEB SEARCH: Search for additional academic sources (fallback)
```

Prioritise sources the researcher already has. Label suggestions with `[ZOTERO]`, `[LOCAL]`, or `[WEB]`.

### Reference List Generation

After drafting is complete, use `/scholar:citation-helper` (Operation 4: Generate Reference List) to:
1. Extract all `[@citekey]` citations from the document
2. Look up each key in the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`)
3. Format as APA 7 reference entries
4. Sort alphabetically
5. Flag any citation keys not found in the Zotero library

### Pandoc Rendering (Optional)

To produce a formatted `.docx` with resolved APA 7 citations, use the Zotero CSL-JSON library and the APA CSL file configured in `.local.md`:
```bash
pandoc paper.md \
  --citeproc \
  --bibliography=<path-to-csl-json-library> \
  --csl=<path-to-apa-csl-file> \
  -o paper.docx
```

## Process

### For Standard Essays
1. **Clarify the brief:** Word count, topic, required theories, citation expectations
2. **Develop thesis:** Clear, arguable, specific
3. **Outline sections:** Each body section = one major argument
4. **Search Zotero library:** Use `/scholar:citation-helper` to find relevant sources for each argument
5. **Draft with citations:** Integrate evidence using `[@citekey]` format throughout
6. **Generate reference list:** Use `/scholar:citation-helper` Operation 4 to build the reference list from cited keys
7. **Build DOCX:** Professional formatting with docx-js (or Pandoc with `--citeproc` for automatic citation rendering)
8. **Verify:** Word count, citation completeness, argument coherence

### For Literature Reviews
1. **Define research question** using PICO or PCC framework
2. **Establish inclusion/exclusion criteria** across 10 categories
3. **Document search strategy** with full Boolean strings
4. **Screen and select studies** (simulate PRISMA flow)
5. **Extract data** into review matrix
6. **Search Zotero library:** Use `/scholar:citation-helper` to get citation keys for all included studies
7. **Synthesise findings** thematically or by outcome, citing with `[@citekey]`
8. **Generate reference list:** Use `/scholar:citation-helper` Operation 4
9. **Write manuscript** following PRISMA 2020 or PRISMA-ScR
10. **Build DOCX** with all required sections and appendices
11. **Verify** completeness against PRISMA checklist

## Reference Files

- `../../references/apa7-guide.md` — Complete APA 7 formatting rules
- `../../references/prisma-checklist.md` — PRISMA 2020 reporting items
- `../../references/review-methodology.md` — Detailed systematic review methodology guide
