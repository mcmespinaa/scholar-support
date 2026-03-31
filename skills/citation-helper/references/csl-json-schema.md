# CSL-JSON Field Reference

The CSL-JSON bibliography file (`references/salsu-library.json`) is an array of item objects. This document describes the fields used by the citation-helper skill.

## Item Structure

```json
{
  "id": "moore.westley2011",
  "type": "article-journal",
  "title": "Surmountable chasms: Networks and social innovation for resilient systems",
  "author": [
    {"family": "Moore", "given": "Michele-Lee"},
    {"family": "Westley", "given": "Frances"}
  ],
  "issued": {"date-parts": [[2011]]},
  "container-title": "Ecology and Society",
  "volume": "16",
  "issue": "1",
  "page": "5",
  "DOI": "10.5751/ES-03812-160105",
  "abstract": "Social innovation is an initiative, product, process...",
  "keyword": "social innovation, resilience, sustainability transitions"
}
```

## Core Fields

| Field | Type | Description | Used In |
|-------|------|-------------|---------|
| `id` | string | Better BibTeX citation key — the value used in `[@citekey]` | All operations |
| `type` | string | Item type (see types below) | Formatting |
| `title` | string | Title of the work | Search, format |
| `author` | array | Authors as `{family, given}` objects | Search, format |
| `issued` | object | Publication date as `{date-parts: [[year, month, day]]}` | Search, format |

## Bibliographic Fields

| Field | Type | Description | Item Types |
|-------|------|-------------|------------|
| `container-title` | string | Journal name, book title (for chapters), or website name | article-journal, chapter, webpage |
| `volume` | string | Journal volume number | article-journal |
| `issue` | string | Journal issue number | article-journal |
| `page` | string | Page range (e.g., `"123-145"`) or article number | article-journal, chapter |
| `DOI` | string | DOI without prefix (e.g., `"10.1234/abc"`) | article-journal, chapter, book |
| `URL` | string | Full URL | All types |
| `publisher` | string | Publisher name | book, chapter, report, thesis |
| `publisher-place` | string | City of publication | book, report |
| `edition` | string | Edition (e.g., `"2"` or `"2nd"`) | book |
| `editor` | array | Editors as `{family, given}` objects | chapter, book |
| `collection-title` | string | Series name | book |
| `number` | string | Report number | report |
| `genre` | string | Type of thesis (e.g., `"Master's thesis"`) | thesis |

## Supplementary Fields

| Field | Type | Description |
|-------|------|-------------|
| `abstract` | string | Abstract text |
| `keyword` | string | Comma-separated Zotero tags |
| `note` | string | Zotero notes |
| `language` | string | Language of the work |
| `ISSN` | string | Journal ISSN |
| `ISBN` | string | Book ISBN |
| `accessed` | object | Date accessed, same format as `issued` |
| `number-of-pages` | string | Total page count |

## Item Types

| CSL Type | Description | APA 7 Format Template |
|----------|-------------|----------------------|
| `article-journal` | Journal article | Author (Year). Title. *Journal*, *Vol*(Issue), Pages. DOI |
| `book` | Book | Author (Year). *Title* (ed.). Publisher. |
| `chapter` | Book chapter | Author (Year). Title. In Editor (Ed.), *Book* (pp. X–X). Publisher. |
| `report` | Report / working paper | Org (Year). *Title* (Report No.). Publisher. URL |
| `webpage` | Website / online page | Author (Year, Month Day). *Title*. Site. URL |
| `thesis` | Dissertation / thesis | Author (Year). *Title* [Type, Institution]. URL |
| `paper-conference` | Conference paper | Author (Year). Title. In *Proceedings* (pp. X–X). Publisher. |
| `article-newspaper` | Newspaper article | Author (Year, Month Day). Title. *Newspaper*. URL |
| `legislation` | Law / regulation | Title, Source § Section (Year). |
| `motion_picture` | Film / video | Director (Year). *Title* [Film]. Studio. |

## Author Object

```json
{
  "family": "Westley",
  "given": "Frances"
}
```

For institutional authors:
```json
{
  "literal": "European Commission"
}
```

## Date Object

```json
// Year only
{"date-parts": [[2023]]}

// Year and month
{"date-parts": [[2023, 6]]}

// Full date
{"date-parts": [[2023, 6, 15]]}
```

Extract year: `item.issued["date-parts"][0][0]`

## Parsing Tips

- The `id` field is the citation key — use it directly in `[@id]`
- Author names may have Unicode characters (é, ö, ñ) — search case-insensitively
- `container-title` may be abbreviated in some exports — search both full and abbreviated forms
- `keyword` is a single comma-separated string, not an array
- `DOI` may or may not include the `https://doi.org/` prefix depending on export settings — normalise before display
- Some fields may be absent — always check for existence before accessing
