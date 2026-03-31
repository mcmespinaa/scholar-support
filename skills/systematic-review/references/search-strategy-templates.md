# Database-Specific Search Strategy Templates

## Table of Contents
1. General Boolean Logic
2. Scopus
3. Web of Science
4. PubMed / MEDLINE
5. ERIC
6. Google Scholar
7. Search Log Template

---

## 1. General Boolean Logic

### Operators

| Operator | Function | Example |
|----------|----------|---------|
| AND | Both terms required (narrows) | "social innovation" AND sustainability |
| OR | Either term (broadens) | farmer OR producer OR grower |
| NOT | Excludes term | agriculture NOT aquaculture |

### Techniques

| Technique | Syntax | Effect |
|-----------|--------|--------|
| Truncation | sustainab* | Matches: sustainable, sustainability, sustain |
| Phrase search | "social enterprise" | Exact phrase only |
| Wildcards | organi?ation | Matches: organization, organisation |
| Proximity | "social W/3 innovation" (Scopus) | Terms within 3 words of each other |
| Nesting | (farmer OR producer) AND (organic OR sustainable) | Groups logical operations |

### Building from PICO/PCC

Structure search strings by combining concept groups:

```
(Population terms)
AND
(Intervention/Concept terms)
AND
(Outcome/Context terms)
```

Each group uses OR to combine synonyms. Groups are combined with AND.

---

## 2. Scopus

### Search interface
- Advanced search: enter full Boolean string
- URL: scopus.com → Search → Documents → Advanced

### Syntax specifics

| Feature | Scopus syntax |
|---------|--------------|
| Field: Title | TITLE("social innovation") |
| Field: Abstract | ABS("social innovation") |
| Field: Title + Abstract + Keywords | TITLE-ABS-KEY("social innovation") |
| Field: Author keywords | AUTHKEY("sustainability") |
| Truncation | * (asterisk) |
| Wildcard | ? (single character) |
| Phrase | "exact phrase" |
| Proximity | W/n (within n words): "social W/3 innovation" |
| Pre-operator | PRE/n (first term before second within n words) |
| Date limit | AND PUBYEAR > 2014 |
| Document type | AND DOCTYPE(ar) — ar = article, re = review |
| Language | AND LANGUAGE(english) |
| Subject area | AND SUBJAREA(SOCI OR BUSI) |

### Example search string (Scopus)

```
TITLE-ABS-KEY(
  ("social innovat*" OR "social enterprise*" OR "social entrepreneur*")
  AND
  ("sustainab*" OR "impact measurement" OR "social impact")
  AND
  ("communit*" OR "local development" OR "civil society")
)
AND PUBYEAR > 2014
AND DOCTYPE(ar OR re)
AND LANGUAGE(english)
```

### Subject area codes (common for social sciences)

| Code | Area |
|------|------|
| SOCI | Social Sciences |
| BUSI | Business, Management, Accounting |
| ECON | Economics |
| ARTS | Arts and Humanities |
| ENVI | Environmental Science |
| DECI | Decision Sciences |

---

## 3. Web of Science

### Search interface
- Advanced search: enter full Boolean string
- URL: webofscience.com → Advanced Search

### Syntax specifics

| Feature | WoS syntax |
|---------|-----------|
| Field: Topic (title + abstract + keywords) | TS=("social innovation") |
| Field: Title | TI=("social innovation") |
| Field: Author | AU=(Smith) |
| Field: Author Keywords | AK=("sustainability") |
| Truncation | * (asterisk) |
| Wildcard | ? (single), $ (zero or one) |
| Phrase | "exact phrase" |
| Proximity | NEAR/n: "social NEAR/3 innovation" |
| Date | Timespan filter in interface, or PY=(2015-2025) |
| Document type | DT=(Article OR Review) |
| Language | LA=(English) |

### Example search string (WoS)

```
TS=(
  ("social innovat*" OR "social enterprise*" OR "social entrepreneur*")
  AND
  ("sustainab*" OR "impact measurement" OR "social impact")
  AND
  ("communit*" OR "local development" OR "civil society")
)
AND PY=(2015-2025)
AND DT=(Article OR Review)
AND LA=(English)
```

### WoS Categories (common for social sciences)

Use WC= to filter by category:
- "Social Sciences Interdisciplinary"
- "Business"
- "Management"
- "Environmental Studies"
- "Sociology"
- "Public Administration"

---

## 4. PubMed / MEDLINE

### Search interface
- Advanced search: use PubMed Advanced Search Builder
- URL: pubmed.ncbi.nlm.nih.gov

### Syntax specifics

| Feature | PubMed syntax |
|---------|--------------|
| Field: Title + Abstract | [tiab] |
| Field: Title | [ti] |
| Field: MeSH Terms | [MeSH Terms] or [mh] |
| Field: All Fields | [all] |
| Truncation | * (asterisk) |
| Phrase | "exact phrase" |
| Date | ("2015/01/01"[Date - Publication] : "2025/12/31"[Date - Publication]) |
| Language | AND English[Language] |
| Article type | AND Journal Article[Publication Type] |
| Humans only | AND Humans[MeSH Terms] |

### MeSH terms

PubMed uses Medical Subject Headings (MeSH) — controlled vocabulary for indexing. Check the MeSH Browser (meshb.nlm.nih.gov) for relevant terms.

### Example search string (PubMed)

```
("social innovation"[tiab] OR "social enterprise"[tiab] OR "social entrepreneurship"[tiab])
AND
("sustainability"[tiab] OR "sustainable development"[tiab] OR "impact measurement"[tiab])
AND
("community"[tiab] OR "local development"[tiab])
AND
("2015/01/01"[Date - Publication] : "2025/12/31"[Date - Publication])
AND
English[Language]
```

---

## 5. ERIC

### Search interface
- Advanced search via ERIC.ed.gov or via EBSCO/ProQuest
- Free access

### Syntax specifics

| Feature | ERIC syntax |
|---------|-----------|
| Field: Title | title:"social innovation" |
| Field: Abstract | abstract:"social innovation" |
| Field: Descriptors (controlled vocab) | descriptor:"Social Change" |
| Truncation | * (asterisk) |
| Phrase | "exact phrase" |
| Date | Publication date filter in interface |
| Peer review | Peer reviewed journals filter in interface |

### Common ERIC descriptors for social sciences

- Social Change
- Social Entrepreneurship
- Sustainability
- Community Development
- Program Effectiveness
- Social Justice
- Innovation

---

## 6. Google Scholar

### Usage notes

Google Scholar is supplementary — use it for:
- Citation tracking (forward/backward)
- Grey literature (theses, reports, working papers)
- Validating completeness of database searches

### Syntax

| Feature | Google Scholar syntax |
|---------|---------------------|
| Phrase | "exact phrase" |
| Exclude | -term |
| Site-specific | site:edu OR site:org |
| Title only | allintitle: social innovation sustainability |
| Author | author:"Smith" |
| Date | Custom date range in interface |

### Limitations

- No Boolean proximity operators
- Cannot export more than ~1000 results
- No controlled vocabulary
- Inconsistent coverage
- Not recommended as a primary database for systematic reviews

---

## 7. Search Log Template

Record the following for each database searched:

| Field | Example |
|-------|---------|
| Database | Scopus |
| Platform | Elsevier |
| Date of search | 2025-03-15 |
| Search string | TITLE-ABS-KEY(("social innovat*") AND ("sustainab*")) AND PUBYEAR > 2014 |
| Filters applied | English, Article or Review, 2015-2025 |
| Results retrieved | 347 |
| Results exported | 347 |
| Export format | RIS / CSV / BibTeX |
| Export filename | scopus_search1_20250315.ris |
| Notes | Broad initial search; will narrow at screening |

### Combining results across databases

After searching all databases:
1. Import all results into a reference manager (Zotero, Mendeley, EndNote)
2. Run automatic deduplication
3. Manual review of remaining potential duplicates
4. Record total before and after deduplication

```
Database 1 (Scopus):     347 results
Database 2 (Web of Science): 289 results
Database 3 (ERIC):        56 results
Database 4 (PubMed):     124 results
Other sources:             12 results
─────────────────────────────────────
Total identified:         828
Duplicates removed:      -215
─────────────────────────────────────
Records for screening:    613
```
