# Common Citation Error Patterns

## Hallucinated References

These patterns indicate a reference may be fabricated (especially common in AI-generated text):

### Red Flags
- Author names that don't appear together in any real publication
- Journal names that don't exist or are subtly misspelled (e.g., "Journal of Sustainable Management" vs. the real "Journal of Sustainability Management")
- DOIs that don't resolve or point to a different article
- Round publication years (2020, 2015) combined with generic-sounding titles
- Page ranges that seem implausible (e.g., pp. 1-45 for a journal article)
- References that perfectly match the essay's argument (too convenient)

### Verification Strategy (4-Tier)
0. Search `references/salsu-library.json` by author `family` name and `issued` year — if found, the reference is confirmed real with authoritative metadata
1. Search `canvas_downloads/` for matching PDFs by author surname or title keywords
2. Search for the exact title in quotes (web)
3. Search for the author + journal combination (web)
4. If DOI present, resolve it at doi.org
5. Check author's real publication list (Google Scholar profile)
6. If no trace found in Zotero, local PDFs, or web, classify as Likely Fabricated

## Ghost Citations

In-text citations with no corresponding reference list entry.

### Common Causes
- Author name spelled differently in-text vs. reference list
- Year discrepancy (e.g., citing 2019 but reference says 2020 — could be online-first vs. print)
- Citation added during revision but reference list not updated
- Copy-paste from another document without bringing the reference

### Detection
- Normalise author surnames (case-insensitive, ignore diacritics) before matching
- Allow ±1 year tolerance when matching, but flag the discrepancy
- Check for common misspellings (e.g., "MacGregor" vs. "McGregor")

## Orphan References

References in the list that are never cited in the text.

### Common Causes
- Reference was cited in a deleted paragraph
- Background reading that was never actually cited
- Copy-pasted reference list from a longer version of the paper

## Misattributed Claims

Claims attributed to a source that doesn't actually support them.

### Common Patterns
- **Topic mismatch:** Citing a paper on leadership for a claim about sustainability metrics
- **Direction mismatch:** Citing a paper that found negative results to support a positive claim
- **Scope mismatch:** Citing a single case study as evidence for a universal trend
- **Methodology mismatch:** Citing a qualitative study for a statistical claim
- **Secondary citation disguised as primary:** Claiming "Smith (2020) found..." when Smith was actually citing Jones (2015)

### Detection Strategy
1. Extract the claim being made
2. Get the source's title, abstract, and keywords
3. Check if the source's domain matches the claim's domain
4. Check if specific numbers/findings could plausibly come from that source
5. Look for qualifier mismatches (source says "may" but text says "definitively")

## APA 7 Format Errors (Ranked by Frequency)

1. **Missing DOI** — DOI exists but not included in reference
2. **Wrong DOI format** — using `doi:` prefix instead of `https://doi.org/`
3. **Ampersand vs. "and"** — `&` in parenthetical, `and` in narrative
4. **et al. misuse** — not using et al. for 3+ authors, or using it for 2 authors
5. **Capitalisation** — title case for article titles (should be sentence case)
6. **Italicisation** — missing italics on journal name/volume or book title
7. **Page number format** — using `p.` for single page, `pp.` for range
8. **Edition format** — `(2nd ed.)` not `(Second Edition)` or `(2nd edition)`
9. **Missing issue number** — issue number omitted when available
10. **URL format** — including "Retrieved from" (APA 6 style, not APA 7)

## Citation Density Benchmarks

| Section | Expected Density | Flag If |
|---------|-----------------|---------|
| Introduction (opening) | Low-moderate | >3 paragraphs with zero citations |
| Introduction (problem statement) | Moderate | >1 paragraph with zero citations |
| Literature review | High | >1 paragraph with zero citations |
| Methods | Low | Uncited methodology claims |
| Results (in a review) | High | Any finding without attribution |
| Discussion | Moderate | >2 paragraphs without citations |
| Conclusion | Low | Specific factual claims without citations |

## Zotero Library Matching

When verifying a reference, always check `references/salsu-library.json` first:

### Search Strategy
1. Read and parse the CSL-JSON file (array of objects)
2. Search by author `family` name (case-insensitive) and `issued` year (`date-parts[0][0]`)
3. If found, compare the document's reference against the CSL-JSON metadata field by field:
   - Authors (`author` array)
   - Title (`title`)
   - Journal (`container-title`)
   - Volume, issue, pages
   - DOI
4. Discrepancies = the document is wrong (Zotero imports from publisher databases)
5. Rate as **Verified (Zotero)** — highest confidence level

### When Zotero Match is Found
- Use the `id` field as the citation key: `[@id]`
- Use metadata as ground truth for correcting errors in the document
- The Zotero entry may have `abstract` and `keyword` fields useful for claim grounding

### When No Zotero Match
- Proceed to local PDFs, then web search
- A reference not being in Zotero does NOT mean it's fabricated — it may just not be in the researcher's collection yet

## Local Source Matching Patterns

When searching `canvas_downloads/` for a reference's source PDF, use these strategies:

### By Author Surname in Filename
Most academic PDFs in the downloads include author names in the filename:
- `Silvius Schipper 2020` → matches `Silvius & Schipper (2020)`
- `Sandberg & Alvesson (2010)` → matches `Sandberg & Alvesson (2011)` (check for year-off-by-one)
- `uhl-bein & arena` → matches `Uhl-Bien & Arena` (case-insensitive, handle misspellings)

**Glob patterns to try (in order):**
1. `**/*Surname1*Surname2*.pdf` (both authors)
2. `**/*Surname1*.pdf` (first author only)
3. `**/*TitleKeyword*.pdf` (distinctive word from title)

### By Course Folder Context
Papers are organised by course, which narrows the search:

| Topic | Look in |
|-------|---------|
| Project management, sustainability in PM | `Project Management and Sustainability-HT25/` |
| Complexity leadership, systems thinking | `Sustainable Development_...Individual, Organisational...-HT25/` |
| Research methods, problematization | `Leadership and Organisation for Sustainability - Research Methods-VT26/` |
| Social entrepreneurship | `Social Entrepreneurship...-VT26/` |
| Organisation theory | `Organising and Leading Sustainable Organisations-HT25/` |

### Handling Mismatches
- **Filename misspellings:** Canvas downloads sometimes have typos (e.g., `uhl-bein` for `Uhl-Bien`). Use fuzzy matching — search for partial surnames.
- **Year discrepancies:** Online-first vs. print publication can differ by 1 year. A local PDF from 2019 may match a reference dated 2020.
- **Lecture slides vs. papers:** Lecture PDFs (e.g., `SALSU 2025 SPM 1.pdf`) may cite the same papers as the reference list. They're useful for context but not for metadata verification — prefer actual article PDFs.
