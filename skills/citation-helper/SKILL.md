---
name: citation-helper
description: "**Zotero Citation Helper**: Searches a Zotero library (CSL-JSON) to find references by author, title, keyword, or year. Returns citation keys [@citekey], formats APA 7 entries, generates reference lists, and suggests citations for claims."
argument-hint: '"search query" [operation: search|cite|format|reflist|suggest]'
user-invokable: true
---

# Zotero Citation Helper

## Overview

This skill connects your Zotero reference library to Claude Code. It reads the auto-exported CSL-JSON bibliography file and provides five operations: search, cite, format, generate reference lists, and suggest citations. No runtime dependencies — it reads a JSON file that Zotero keeps updated automatically via Better BibTeX.

## When to Use This Skill

Use when the user asks you to:
- Find or search references on a topic, by author, or by year
- Get a citation key for a known article or author
- Format a reference entry in APA 7
- Generate a reference list from `[@citekey]` citations in a document
- Suggest citations for a claim or topic sentence
- Look up what's in their Zotero library
- Search for sources to cite in an essay or paper

## Input Requirements

**Primary data source:** The Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`).

If this file does not exist, instruct the user to set up Zotero auto-export. Refer them to `references/zotero-setup.md` in this skill's references directory and provide a brief summary:

> Your Zotero library isn't connected yet. You need to:
> 1. Install Zotero 7 and the Better BibTeX plugin
> 2. Create a SALSU collection in Zotero
> 3. Right-click the collection → Export → Better CSL JSON → check "Keep updated" → save to your configured CSL-JSON path
>
> See `references/zotero-setup.md` for the full setup guide.

## Operations

### Operation 1: Search Library

**Triggers:** "find references about X", "search library for X", "who wrote about X", "what do I have on X"

**Process:**
1. Read and parse the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`) (array of CSL-JSON objects)
2. Search across these fields (case-insensitive):
   - `author` — match against `family` and `given` name fields
   - `title` — article/book title
   - `abstract` — if available
   - `container-title` — journal or book name
   - `issued` — year (from `date-parts[0][0]`)
   - `note` — Zotero notes
   - `keyword` — Zotero tags
3. Multiple search terms are treated as AND (all must match across any fields)
4. Return results sorted by relevance (title/author matches ranked higher than abstract matches)

**Output format per result:**
```
[@citekey] — Author, A. A., & Author, B. B. (Year). Title of article. *Journal Name*, *Volume*(Issue), Pages.
   Abstract: First 150 characters of abstract...
   Tags: tag1, tag2
```

Show up to 10 results. If more exist, tell the user the total count and suggest narrowing the search.

### Operation 2: Get Citation Key

**Triggers:** "cite Moore and Westley", "citekey for X", "citation key for X", "how do I cite X"

**Process:**
1. Search the library for the specified reference (by author + year, title, or selection from search results)
2. Return the Better BibTeX citation key from the `id` field of the CSL-JSON item

**Output formats:**
| Context | Format | Example |
|---------|--------|---------|
| Single parenthetical | `[@citekey]` | `[@moore.westley2011]` |
| Multiple sources | `[@key1; @key2]` | `[@moore.westley2011; @avelino.etal2019]` |
| Narrative | `@citekey` | `@moore.westley2011` |
| With page number | `[@citekey, p. 15]` | `[@moore.westley2011, p. 15]` |
| Suppress author | `[-@citekey]` | `[-@moore.westley2011]` |

Explain the Pandoc citation syntax briefly when returning keys, so the user knows how to use them.

### Operation 3: Format APA 7 Reference Entry

**Triggers:** "format reference for X", "APA reference for X", "how should I reference X"

**Process:**
1. Find the item in the CSL-JSON library
2. Determine the item type (`article-journal`, `book`, `chapter`, `report`, `webpage`, `thesis`)
3. Apply APA 7 formatting rules from `../../references/apa7-guide.md`

**Formatting rules by type:**

**Journal article:**
```
Author, A. A., Author, B. B., & Author, C. C. (Year). Title of article in sentence case. *Journal Name*, *Volume*(Issue), Pages. https://doi.org/xxxxx
```

**Book:**
```
Author, A. A. (Year). *Title of book in sentence case* (Xth ed.). Publisher.
```

**Book chapter:**
```
Author, A. A. (Year). Title of chapter. In B. B. Editor (Ed.), *Title of book* (pp. xx–xx). Publisher.
```

**Report:**
```
Organisation. (Year). *Title of report* (Report No. xxx). Publisher. https://url
```

**Webpage:**
```
Author, A. A. (Year, Month Day). *Title of page*. Site Name. https://url
```

**Key formatting details:**
- Author names: `Surname, A. A.` with `&` before last author
- Use `et al.` only in in-text citations, never in reference list (list all authors up to 20)
- Sentence case for article/chapter titles (capitalise only first word, first word after colon, and proper nouns)
- Title case for journal names
- Italicise journal names, volume numbers, book/report titles
- DOI as `https://doi.org/` hyperlink
- No "Retrieved from" before URLs

### Operation 4: Generate Reference List from Document

**Triggers:** "generate reference list", "make references for my paper", "reference list from citations", "what references does my document need"

**Process:**
1. Read the specified markdown document (or accept pasted text)
2. Extract all Pandoc citation keys using regex:
   - `[@citekey]` — standard parenthetical
   - `[@key1; @key2; @key3]` — multiple sources
   - `@citekey` — narrative (word boundary: preceded by space or start of line)
   - `[-@citekey]` — author-suppressed
   - `[@citekey, p. XX]` — with locator
3. Deduplicate the extracted keys
4. Look up each key in the Zotero CSL-JSON library configured in the plugin's `.local.md` file (default: `references/salsu-library.json`)
5. Format each as an APA 7 reference entry (Operation 3)
6. Sort alphabetically by first author surname
7. Output the complete reference list

**Handle errors:**
- Keys not found in library → list separately with a warning:
  ```
  ⚠ Citation keys not found in Zotero library:
    - [@unknown.key2024] — not in library. Add this item to your Zotero collection.
  ```
- Duplicate keys → deduplicate silently
- Malformed keys → flag with the location in the document

### Operation 5: Suggest Citations

**Triggers:** "suggest citation for X", "what can I cite for X", "find me a source on X", "I need a reference for X"

**Process:**
1. Parse the user's claim, topic sentence, or concept
2. Extract key terms (remove stop words, identify domain terms)
3. Search the library (Operation 1) using those terms
4. Return 1-5 relevant matches with:
   - Citation key in Pandoc format
   - Full APA 7 reference
   - Brief explanation of why this source is relevant (based on title/abstract match)

**If no matches found:**
> No matching sources found in your Zotero library for "[search terms]".
> Consider searching for sources on this topic and adding them to your Zotero collection.

## Human-in-the-Loop: Source Verification Protocol

**CRITICAL RULE: Never cite a source you have not read or verified.**

Every source cited must meet one of these conditions:
1. The fulltext has been read by Claude Code (PDF, HTML, or pasted text)
2. The user confirms they have read it and provides key claims to cite
3. The source is from the user's own prior work (e.g., Espina, 2026)

Sources found only by metadata (title, abstract, DOI) are candidates, not citable sources, until verified.

### When Writing: The Verification Loop

1. **SEARCH:** Find candidate sources in Zotero library
2. **CHECK FULLTEXT:** For each candidate, check local directories first:
   - The local PDFs directory configured in `.local.md` for course materials and assigned readings
   - Any additional research directories the user has specified
   - Check if the source URL is openly accessible
   - Check the user's NotebookLM resources
3. **IF FULLTEXT FOUND:** Read it. Verify the claim matches what the source actually says. Cite with confidence.
4. **IF FULLTEXT NOT FOUND:** Ask the user to provide it:
   - "I want to cite [Author (Year)] for [specific claim], but I don't have access to the fulltext. Could you download the PDF and save it to Downloads/Academic Research/, paste the relevant section, or confirm the claim from your reading?"
5. **IF BEHIND PAYWALL:** Tell the user explicitly with the DOI link and suggest accessing through MAU library at https://libguides.mau.se/leadership_sustainability
6. **NEVER** fabricate claims, page numbers, or quotes from sources not read.

### Verified vs. Candidate Sources

When suggesting citations, clearly distinguish:
- **VERIFIED** (fulltext read): Show citation key, formatted reference, specific claim with page number
- **CANDIDATE** (metadata only): Show citation key and reference, mark with a warning, ask the user to provide fulltext before citing

### Handling Missing Sources During Writing

1. **Pause writing** at the point where the unverified citation is needed
2. **List what is missing** with DOIs and the specific claim each source is needed for
3. **Continue writing** sections that do not depend on the missing sources
4. **Resume** once the user provides the fulltexts

## Integration with Other Skills

### With academic-essay / research-proposal / systematic-review

When writing text that needs citations:
1. The writing skill drafts content
2. For claims needing sources, invoke citation-helper to search the library
3. **Verify fulltext access** before inserting any citation (see Human-in-the-Loop above)
4. Insert the returned `[@citekey]` at the appropriate location
5. After the document is complete, run Operation 4 to generate the reference list
6. **Final verification pass:** confirm every cited source has been read or user-confirmed

### With citation-checker

The checker can cross-reference cited sources against the Zotero library:
- References in the paper that exist in Zotero → higher confidence they're real
- References NOT in Zotero → flag for verification (may still be valid, just not in the library)
- Flag any citation where fulltext was never accessed during the writing session

### With citation-fixer

When the fixer needs to correct metadata (wrong DOI, author names, etc.):
- Look up the correct data from the CSL-JSON file (Zotero imports metadata from authoritative databases)
- The Zotero library serves as a ground-truth source for reference metadata

## Pandoc Rendering (Optional)

If the user wants to render a markdown document with resolved citations to DOCX or PDF:

```bash
# Paths are configured in the plugin's .local.md file.
# Defaults: BIBLIOGRAPHY=references/salsu-library.json, CSL_FILE=references/apa.csl
pandoc paper.md \
  --citeproc \
  --bibliography=$BIBLIOGRAPHY \
  --csl=$CSL_FILE \
  -o paper.docx
```

This replaces all `[@citekey]` with formatted APA 7 citations and appends a reference list automatically. Requires Pandoc to be installed (`brew install pandoc`).

## CSL-JSON Parsing Reference

The CSL-JSON file is an array of objects. Key fields to parse:

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Better BibTeX citation key |
| `type` | string | `article-journal`, `book`, `chapter`, `report`, `webpage`, `thesis` |
| `title` | string | Full title |
| `author` | array | `[{family: "Surname", given: "First M."}]` |
| `issued` | object | `{date-parts: [[2023]]}` or `{date-parts: [[2023, 6, 15]]}` |
| `container-title` | string | Journal name or parent book title |
| `volume` | string | Journal volume |
| `issue` | string | Journal issue |
| `page` | string | Page range, e.g. `"123-145"` |
| `DOI` | string | DOI without `https://doi.org/` prefix |
| `URL` | string | Full URL |
| `abstract` | string | Abstract text |
| `publisher` | string | Publisher name |
| `edition` | string | Edition |
| `editor` | array | `[{family, given}]` for edited books |
| `keyword` | string | Comma-separated Zotero tags |

See `references/csl-json-schema.md` for the complete field reference.

## Reference Files

- `references/zotero-setup.md` — How to install and configure Zotero + Better BibTeX for this workflow
- `references/csl-json-schema.md` — Complete CSL-JSON field reference for parsing the bibliography file
- Shared: `../../references/apa7-guide.md` — APA 7 formatting rules
