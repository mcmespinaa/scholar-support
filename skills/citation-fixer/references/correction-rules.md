# Citation Correction Rules

## Auto-Fix Transformation Patterns

### 1. Formatting Artifacts

| Pattern | Fix | Example |
|---------|-----|---------|
| Backslash before `)` | Remove `\` | `2007\)` → `2007)` |
| Backslash before `.` | Remove `\` | `115183\.` → `115183.` |
| Escaped characters in citations | Remove escape character | `et al\.,` → `et al.,` |
| Double spaces | Collapse to single | `Author,  A.` → `Author, A.` |

### 2. DOI Format

| Pattern | Fix |
|---------|-----|
| `doi:10.XXXX/YYYY` | `https://doi.org/10.XXXX/YYYY` |
| `http://dx.doi.org/10.XXXX/YYYY` | `https://doi.org/10.XXXX/YYYY` |
| `http://doi.org/10.XXXX/YYYY` | `https://doi.org/10.XXXX/YYYY` |
| `DOI: 10.XXXX/YYYY` | `https://doi.org/10.XXXX/YYYY` |
| Bare DOI without prefix | Prepend `https://doi.org/` |

### 3. Ampersand / "and" Rules

| Context | Rule | Example |
|---------|------|---------|
| Parenthetical citation | Use `&` | `(Smith & Jones, 2020)` |
| Narrative citation | Use `and` | `Smith and Jones (2020) argue...` |
| Reference list | Use `&` before last author | `Smith, A., Jones, B., & Lee, C.` |

**Detection:** If citation is inside `(...)`, it's parenthetical. If author names precede `(Year)`, it's narrative.

### 4. et al. Rules

| Authors | Rule | Example |
|---------|------|---------|
| 1 author | Use surname only | `(Smith, 2020)` |
| 2 authors | Always list both | `(Smith & Jones, 2020)` |
| 3+ authors | Use et al. from first citation | `(Smith et al., 2020)` |

**Common errors to fix:**
- `et al` without period → `et al.`
- `et. al.` → `et al.`
- `Et al.` at start of sentence → acceptable (capitalised)
- Using et al. for 2 authors → list both names

### 5. Capitalisation

**Article/chapter titles — sentence case:**
- Capitalise only first word, first word after colon, and proper nouns
- `The Effects of Climate Change on Agriculture` → `The effects of climate change on agriculture`

**Journal names — title case:**
- Capitalise all major words
- `journal of business research` → `Journal of Business Research`

**Do NOT change:**
- Acronyms (`AI`, `SDG`, `PRISMA`)
- Proper nouns within titles
- Journal names that have unusual official capitalisation

### 6. Italicisation (Markdown)

| Element | Format | Example |
|---------|--------|---------|
| Journal name | `*Journal Name*` | `*Journal of Business Research*` |
| Volume number | `*Volume*` | `*189*` |
| Book title | `*Book Title*` | `*Thinking in systems: A primer*` |
| Issue number | NOT italicised | `(2)` in plain text |

### 7. Edition Format

| Wrong | Correct |
|-------|---------|
| `(Second Edition)` | `(2nd ed.)` |
| `(2nd edition)` | `(2nd ed.)` |
| `(Third Ed.)` | `(3rd ed.)` |
| `(1st Edition)` | Remove — don't note first editions |

### 8. URL and Retrieval

| Wrong | Correct |
|-------|---------|
| `Retrieved from https://...` | `https://...` |
| `Retrieved on March 1, 2025, from https://...` | `https://...` |
| `Available at: https://...` | `https://...` |

### 9. Punctuation Placement

| Wrong | Correct | Rule |
|-------|---------|------|
| `...important.  (Smith, 2020)` | `...important (Smith, 2020).` | Period after parenthetical citation |
| `...important (Smith, 2020.)` | `...important (Smith, 2020).` | Period outside parentheses |
| `...important" (Smith, 2020, p. 5).` | Correct as-is | Quote + citation + period |

### 10. Page Numbers

| Wrong | Correct | When |
|-------|---------|------|
| `page 5` | `p. 5` | Single page |
| `pages 5-10` | `pp. 5–10` | Page range (use en dash) |
| `p.5` | `p. 5` | Space after `p.` |
| `pp.5-10` | `pp. 5–10` | Space + en dash |

## Safe Auto-Fix Boundaries

### Safe to Fix Automatically

These changes are mechanical and cannot alter meaning:

- Formatting artifacts (backslashes, escaped characters)
- DOI format standardisation
- Adding missing DOIs (confirmed via web search)
- Correcting volume/issue/pages (confirmed against publisher database)
- Correcting author lists (confirmed against publisher database)
- Ampersand/and in correct context
- et al. usage
- Capitalisation of titles (sentence case / title case)
- Italicisation markup
- Edition format
- "Retrieved from" removal
- Punctuation placement
- Page number format
- Alphabetical reordering of reference list

### Requires Human Review (NEVER auto-fix)

These changes involve judgment about content, meaning, or scholarly integrity:

| Issue | Why | What to do instead |
|-------|-----|-------------------|
| Fabricated reference | Deleting a reference changes the argument | Flag + provide closest real match |
| Misattributed claim | The text may need rewriting, not just a citation swap | Flag + explain the mismatch |
| Orphan reference | User may want to add a citation for it | Flag + ask user |
| Missing citation (content) | Only the author knows the intended source | Insert `[CITATION NEEDED]` marker + suggest sources |
| Over-reliance on source | Structural argument decision | Insert `[DIVERSIFY SOURCES]` marker |
| Citation desert | Author must decide what to cite | Insert `[CITATION DESERT]` marker |
| Uncertain metadata | Conflicting sources for correct metadata | Flag both options + ask user |

## Gap Marker Conventions

### Marker Format

All markers use square brackets and caps for visibility:

```
[CITATION NEEDED]
[CITATION DESERT — consider adding sources]
[DIVERSIFY SOURCES]
```

### Marker Placement

- `[CITATION NEEDED]` — insert immediately after the uncited claim, before the period
- `[CITATION DESERT]` — insert as a new line at the end of the section/paragraph
- `[DIVERSIFY SOURCES]` — insert after the over-cited source's last appearance in the paragraph

### Source Suggestions

When suggesting sources for gap markers, follow the 4-tier lookup priority and format as:

```markdown
<!-- Suggested sources:
- [ZOTERO] Author, A. (Year). Title. Use: [@citekey]
- [LOCAL] Author, B. (Year). Title. Found at: canvas_downloads/Course/file.pdf
- [NOTEBOOKLM] Author, C. (Year). Title. From notebook: Notebook Name
- [WEB] Author, D. (Year). Title. Journal. https://doi.org/...
-->
```

Use HTML comments so suggestions are invisible in rendered markdown but visible in the source file.

## Local Source Priority

### Zotero as Ground Truth (Tier 0)

The Zotero library (`references/salsu-library.json`) is the **highest-priority** source for metadata corrections:

| Situation | Action |
|-----------|--------|
| Reference found in Zotero with different metadata | **Use Zotero's values** — imported from publisher databases |
| DOI missing in document but present in Zotero | Add Zotero's DOI |
| Author names differ between document and Zotero | Use Zotero's author list |
| Volume/issue/pages differ | Use Zotero's values |
| Reference NOT in Zotero | Proceed to local PDFs, then web search |

**Search strategy:** Parse `references/salsu-library.json`, search by author `family` name and `issued` year (`date-parts[0][0]`). The `id` field provides the citation key for `[@citekey]` suggestions.

### When to Prefer Local Metadata (Tier 1)

Local PDFs in `canvas_downloads/` are the second most reliable source for metadata correction:

| Situation | Local vs. Web |
|-----------|--------------|
| PDF found locally with clear title page | **Always prefer local** — read first pages for exact metadata |
| PDF found but it's a lecture slide, not the paper | Use for context only, prefer web for metadata |
| PDF filename contains author + year but is a scan | Read first page; if readable, prefer local |
| Multiple web results conflict | Local PDF breaks the tie if available |
| Reference is very recent (2025) and web has sparse results | Local PDF is especially valuable |

### Matching References to Local PDFs

Use the patterns from `citation-checker/references/verification-patterns.md` — "Local Source Matching Patterns" section. Key strategies:
1. Glob by author surname: `canvas_downloads/**/*Surname*.pdf`
2. Glob by title keyword: `canvas_downloads/**/*keyword*.pdf`
3. Narrow by course folder when the topic is known
4. Handle filename misspellings with partial matches
5. Allow ±1 year tolerance for online-first vs. print dates

## Bottom-to-Top Editing Strategy

When applying multiple fixes to a document, always apply changes from the highest line number to the lowest. This ensures that earlier line numbers remain valid as edits are applied.

**Order of operations within the same line:**
1. Metadata corrections (change content)
2. Format fixes (change presentation)
3. Gap markers (add new text)

If an edit changes the length of a line (adding a DOI, inserting a marker), all subsequent edits at lower line numbers remain unaffected because they haven't been reached yet.
