---
name: project-report
description: "**Academic Project Report Writer**: Creates structured project reports with business model canvases, concept notes, and theory-integrated sections. Professional .docx output with APA 7 citations, tables, and formatting."
argument-hint: '"project topic" [framework: BMC|ToC|SWOT|PESTEL] [word-count]'
user-invokable: true
---

# Academic Project Report Skill

## Overview

This skill produces professional academic project reports as `.docx` files. These reports typically combine visual analytical frameworks (e.g., Sustainable Business Model Canvas) with structured written concept notes grounded in theory and supported by APA 7 citations. The skill is designed for university-level coursework in social entrepreneurship, social innovation, and sustainability, but adapts to any academic context requiring structured project documentation.

## When to Use This Skill

Use when the user asks you to:
- Write, rewrite, or improve an academic project report
- Create a concept note with theoretical grounding
- Produce a Sustainable Business Model Canvas with prose explanation
- Combine visual frameworks with structured academic writing
- Format a submission that needs APA 7 citations, headings, tables, and professional layout

## Report Architecture

A typical project report has two major parts. Adapt based on the user's specific assignment requirements.

### Part A: Analytical Framework (Visual)

Usually a Business Model Canvas or Sustainable BMC presented as a formatted table on a landscape page, followed by a prose explanation (~300-500 words) that walks through the canvas blocks.

**Standard Sustainable BMC blocks:**

| Row | Blocks |
|-----|--------|
| Top | Key Partners, Key Activities, Value Propositions, Customer Relationships, Customer Segments |
| Middle | Key Resources, Channels, Revenue & Cost Structure |
| Bottom | Social Sustainability, Environmental Sustainability, Economic Sustainability |

**Canvas Explanation** should cover:
1. Core value proposition and what differentiates it
2. Customer segments and their roles
3. Key partners and why they matter
4. Revenue model and cost structure
5. Sustainability dimensions (social, environmental, economic)

### Part B: Concept Note (Structured Text)

The concept note is the analytical core. Typical structure (adapt to assignment):

| Section | Content | Word Target |
|---------|---------|-------------|
| 1. Need / Demand / Relevance | Problem analysis with data and CLA/iceberg layers | 500-700 |
| 2. Concept & Business Model | Initiative description, pillars, revenue model, differentiator | 500-700 |
| 3. Sustainability Impact | Theory of Change, short/medium/long-term outcomes, SDG alignment | 400-600 |
| 4. Impact Measurement | Mixed methods approach, indicators table, communication channels | 400-500 |
| 5. Theoretical Reflection | 1-2 course theories applied to the initiative | 300-400 |

**Total target: 2,000-3,000 words** (excluding references, tables, canvas)

### Supporting Elements

- **Project Overview** (~200 words): Mission, problem, concept, scope, constraints, SMART objectives
- **Reference List**: APA 7 format with hanging indent
- **Tables**: Impact indicators, stakeholder mapping, etc.

## Writing Quality Standards

### Theory Integration
Every section should connect theory to practice. Use the theory-to-section mapping pattern:

- **Section 1**: CLA/Iceberg (problem layers), Problem Tree (root causes), Systems Thinking (complexity)
- **Section 2**: Sustainable BMC (Evans et al.), CoP theory, System Leadership (Senge et al.)
- **Section 3**: Theory of Change, Transformative SI (Avelino et al.), Game Changers (Westley et al.)
- **Section 4**: Mixed methods assessment (Kickul & Lyons), Social innovation outcomes (Lindberg et al.)
- **Section 5**: Primary theoretical lens + supporting theory, applied to the specific initiative

### Citation Style (APA 7)

| Situation | Format |
|-----------|--------|
| 1 author | (Author, Year) |
| 2 authors | (Author & Author, Year) |
| 3+ authors | (Author et al., Year) |
| Organisation (first) | (Organisation [Abbrev], Year) |
| Organisation (subsequent) | (Abbrev, Year) |
| Direct quote | (Author et al., Year, p. XX) |
| Narrative | Author et al. (Year) argue that... |

### Evaluation Criteria Alignment

Structure writing to satisfy these common evaluation dimensions:
1. **Clarity and depth** of problem analysis
2. **Use of relevant theory** in written sections
3. **Originality and feasibility** of the proposed concept
4. **Coherence** between problem, concept, impact logic, and measurement
5. **Clarity of communication** (structure, readability, flow)

## Document Generation

Use the `docx` skill's infrastructure for creating the Word document. Key technical patterns:

### Page Setup
```javascript
// Portrait for text sections
{ width: 12240, height: 15840, margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 } }

// Landscape for BMC canvas
{ width: 15840, height: 12240, margin: { top: 720, right: 720, bottom: 720, left: 720 } }
```

### Color Palette (Professional Academic)
```javascript
const PRIMARY = "1B4332";    // Deep green headers
const SECONDARY = "2D6A4F";  // Medium green subheaders
const ACCENT = "40916C";     // Light green accents
const GRAY = "666666";       // Meta text, headers/footers
```

### Document Sections (as separate page sections)
1. **Title Page** (portrait, no header/footer)
2. **Project Overview** (portrait, with header)
3. **Part A: BMC** (landscape, with header)
4. **Part B: Concept Note** (portrait, with header and page numbers)

### BMC Table Pattern
- Use 5-column table for top row (Partners, Activities, VP, Relationships, Segments)
- Use 3-column table for middle row (Resources, Channels, Revenue/Cost)
- Use 3-column table for sustainability row (Social, Environmental, Economic)
- Dark header cells with white text, content cells with bullet points

### Body Text
- Font: Arial, size 24 half-points (12pt)
- Paragraph spacing: 200 after
- Headings: H1 for parts, H2 for sections, H3 for subsections

### Reference List
- Hanging indent: `indent: { left: 720, hanging: 720 }`
- Font size slightly smaller (22 half-points / 11pt)

## Process

1. **Read the assignment brief** carefully. Identify required sections, word limits, evaluation criteria.
2. **Gather project content** from user's existing documents, working docs, transcripts.
3. **Map theory to sections** using the theory-to-section matrix.
4. **Draft each section** with proper citations and data support.
5. **Build the DOCX** using docx-js with professional formatting.
6. **Validate** word count, citation completeness, and structural coherence.
7. **Save** to workspace folder and present to user.

## Reference: Common Theoretical Frameworks

Read `references/theory-frameworks.md` for detailed descriptions of commonly used frameworks in social entrepreneurship and innovation courses, including CLA, Theory of Change, Systems Thinking, Sustainable BMC, Communities of Practice, and Multi-Level Perspective.

## Reference: APA 7 Quick Guide

Read `../../references/apa7-guide.md` for comprehensive APA 7 formatting rules including in-text citations, reference list formatting for journals, books, chapters, websites, and reports.
