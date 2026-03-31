# Systematic Literature Review Methodology Guide

## Table of Contents
1. Research Question Development (PICO/PCC)
2. Search Strategy Design
3. Study Selection Process
4. Data Extraction
5. Quality Assessment
6. Data Synthesis
7. Common Pitfalls

---

## 1. Research Question Development

### PICO Framework (Systematic Reviews)

| Element | Question | Example |
|---------|----------|---------|
| **P** Population | Who are the relevant participants? | Adults with chronic pain |
| **I** Intervention | What management strategy/test/exposure? | Mindfulness-based therapy |
| **C** Comparison | What is the control or alternative? | Cognitive behavioural therapy |
| **O** Outcome | What patient-relevant consequences? | Pain reduction, quality of life |

**Optional T (Time):** What is the time horizon for demonstrating the outcome?

### PCC Framework (Scoping Reviews)

| Element | Question | Example |
|---------|----------|---------|
| **P** Population | Who is being studied? | Healthcare workers |
| **C** Concept | What is the key concept? | Burnout and resilience |
| **C** Context | What is the setting? | Hospital settings in Nordic countries |

### Five Question Types

1. **Therapy:** "In [P], does [I] compared with [C] improve [O]?"
2. **Etiology:** "In [P], does exposure to [I] increase risk of [O]?"
3. **Diagnosis:** "In [P], how accurate is [I] compared with [C] for detecting [O]?"
4. **Prevention:** "In [P], does [I] reduce incidence of [O] compared with [C]?"
5. **Prognosis:** "In [P] with condition [I], what is the likelihood of [O] over [T]?"

---

## 2. Search Strategy Design

### Boolean Operators

| Operator | Function | Example |
|----------|----------|---------|
| AND | Narrows (both terms required) | "organic food" AND "market access" |
| OR | Broadens (either term) | farmer OR producer OR grower |
| NOT | Excludes | agriculture NOT aquaculture |

### Search Components

Build search strings from PICO elements:
```
(Population terms OR synonyms)
AND
(Intervention terms OR synonyms)
AND
(Outcome terms OR synonyms)
```

### Search Techniques

- **Truncation (*):** farm* finds farmer, farming, farmland
- **Phrase search (""):** "social innovation" finds exact phrase
- **MeSH/Subject headings:** Use controlled vocabulary where available
- **Proximity operators:** "organic NEAR/3 market" finds terms within 3 words

### Database Search Log

Record for each database:
- Database name and platform (e.g., Scopus via Elsevier)
- Date of search execution
- Complete search string
- Number of results retrieved
- Export format and filename

---

## 3. Study Selection Process

### Two-Stage Screening

**Stage 1: Title & Abstract Screening**
- Apply inclusion/exclusion criteria
- When in doubt, include for full-text review
- Ideally: 2 independent reviewers, resolve disagreements by discussion

**Stage 2: Full-Text Review**
- Retrieve full texts of all potentially eligible studies
- Apply detailed eligibility criteria
- Document reasons for exclusion
- Resolve disagreements by discussion or third reviewer

### Inclusion/Exclusion Criteria (10 Categories)

1. **Date:** Publication year range (e.g., 2015-2025)
2. **Exposure/Intervention:** Specific intervention or topic focus
3. **Geographic location:** Country or region restrictions (if any)
4. **Language:** English only? Include translations?
5. **Participants/Population:** Age, condition, demographic criteria
6. **Peer review:** Published in peer-reviewed journals only?
7. **Outcomes:** Must report specific outcome measures
8. **Setting:** Clinical, community, workplace, etc.
9. **Study design:** RCTs only? Include observational? Qualitative?
10. **Publication type:** Journal articles, conference papers, grey literature?

---

## 4. Data Extraction

### Review Matrix Template

| Field | Description |
|-------|-------------|
| Author(s) | All authors (or first author et al.) |
| Year | Publication year |
| Study Design | RCT, cohort, cross-sectional, qualitative, etc. |
| Population | Sample characteristics (n, demographics) |
| Intervention/Exposure | What was studied |
| Comparison | Control or comparator |
| Outcomes | Primary and secondary outcomes measured |
| Key Findings | Main results with effect sizes where applicable |
| Quality Score | Result of critical appraisal |
| Notes | Limitations, funding, conflicts |

### Tips for Data Extraction
- Pilot the extraction form on 2-3 studies first
- Extract data independently (ideally 2 reviewers)
- Contact authors for missing data where feasible
- Document all assumptions and decisions

---

## 5. Quality Assessment

### Common Tools by Study Design

| Study Design | Assessment Tool |
|-------------|----------------|
| RCTs | Cochrane Risk of Bias tool (RoB 2) |
| Observational (cohort) | Newcastle-Ottawa Scale (NOS) |
| Observational (cross-sectional) | JBI Checklist |
| Qualitative | CASP Qualitative Checklist |
| Mixed methods | Mixed Methods Appraisal Tool (MMAT) |
| Scoping reviews | Optional (JBI checklist if used) |

### Risk of Bias Domains (RoB 2)
1. Randomisation process
2. Deviations from intended interventions
3. Missing outcome data
4. Measurement of the outcome
5. Selection of the reported result

---

## 6. Data Synthesis

### Narrative Synthesis
- Organise studies thematically or by outcome
- Compare and contrast findings across studies
- Identify patterns, contradictions, and gaps
- Discuss potential explanations for heterogeneity

### Quantitative Synthesis (Meta-analysis)
- Calculate pooled effect estimates (if studies sufficiently homogeneous)
- Report heterogeneity (I-squared, Cochran's Q)
- Use forest plots for visual presentation
- Conduct sensitivity and subgroup analyses where appropriate

### Reporting Results
- Use PRISMA flow diagram
- Present study characteristics in summary table
- Display individual study results before pooled estimates
- Report confidence intervals for all estimates

---

## 7. Common Pitfalls

1. **Vague research question:** Use PICO/PCC to sharpen focus
2. **Incomplete search:** Search at least 2-3 databases with full strategy
3. **Selection bias:** Use independent dual screening where possible
4. **Narrative rather than synthesis:** Analyse patterns, don't just summarise each study
5. **Ignoring quality:** Always assess and discuss study quality
6. **Publication bias:** Acknowledge and address (funnel plots, trim-and-fill)
7. **Overgeneralising:** Be cautious about extrapolating beyond the evidence
8. **Missing PRISMA items:** Use the checklist systematically

---

## Reference Standards

- **PRISMA 2020:** Page, M. J., et al. (2021). The PRISMA 2020 statement. BMJ.
- **PRISMA-ScR:** Tricco, A. C., et al. (2018). PRISMA Extension for Scoping Reviews. Annals of Internal Medicine.
- **JBI Manual:** Aromataris, E., & Munn, Z. (Eds.). (2020). JBI Manual for Evidence Synthesis.
- **Arksey & O'Malley:** Arksey, H., & O'Malley, L. (2005). Scoping studies: towards a methodological framework. International Journal of Social Research Methodology.
- **PROSPERO:** International prospective register of systematic reviews. https://www.crd.york.ac.uk/prospero/
