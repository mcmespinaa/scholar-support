# Scholar Plugin — Local Configuration

Configure these paths to match your local setup. Copy this file to `.local.md` in the plugin root and update the values.

## Zotero Library

Path to your auto-exported CSL-JSON bibliography file (from Better BibTeX):

```
ZOTERO_LIBRARY: references/salsu-library.json
```

Setup: Install Zotero 7 + Better BibTeX → create a collection → right-click → Export → Better CSL JSON → check "Keep updated" → save to this path. See `skills/citation-helper/references/zotero-setup.md` for the full guide.

## APA Citation Style

Path to the APA 7 CSL file (for Pandoc rendering):

```
CSL_FILE: references/apa7.csl
```

Download from: https://github.com/citation-style-language/styles/blob/master/apa.csl

## Local PDFs

Path to a directory containing downloaded academic PDFs (course materials, articles, etc.). Used by citation-checker and citation-fixer for full-text verification.

```
LOCAL_PDFS: canvas_downloads/
```

This can be any directory — the skills will search it recursively for PDFs matching author surnames and title keywords.

## Voice Profiles (Optional)

The academic-editor skill (Pass 3: Voice Editor) uses author voice profiles. Place `.md` files in `skills/academic-editor/references/voice-profiles/` to configure author-specific editing.

## NotebookLM (Optional)

If you use Google NotebookLM for source annotation, the citation skills will query your notebooks as Tier 2 in the lookup chain. No configuration needed — it uses the `/notebooklm` skill if available.
