# Zotero + Better BibTeX Setup Guide

## Prerequisites

- macOS (tested on Darwin)
- Zotero 7 (free, open-source reference manager)
- Better BibTeX plugin for Zotero

## Step 1: Install Zotero 7

1. Download from https://www.zotero.org/download/
2. Install and launch Zotero
3. Create a free Zotero account at https://www.zotero.org/user/register (enables cloud sync)
4. In Zotero: Edit → Settings → Sync → sign in with your account

## Step 2: Install Better BibTeX Plugin

1. Download the latest `.xpi` file from https://github.com/retorquere/zotero-better-bibtex/releases/latest
2. In Zotero: Tools → Add-ons → gear icon → Install Add-on From File → select the `.xpi`
3. Restart Zotero when prompted

### Configure Citation Key Format

After installing Better BibTeX:

1. In Zotero: Edit → Settings → Better BibTeX → Citation keys
2. Set the citation key format to:
   ```
   authEtal2.lower + year
   ```
   This produces readable keys like:
   - `moore.westley2011` (2 authors)
   - `avelino.etal2019` (3+ authors)
   - `senge2015` (1 author)

3. Click "Refresh" to regenerate keys for existing items

## Step 3: Organise Your Library

Create a collection structure that mirrors your programme:

```
My Library
└── SALSU
    ├── OL640E Sustainability Leadership
    ├── OL641E Social Entrepreneurship
    ├── OL642E Project Management
    ├── OL652E Research Methods
    ├── OL653E Master Thesis
    └── Shared / Cross-cutting
```

To create collections: right-click "My Library" → New Collection

### Adding Items to Zotero

- **Browser connector:** Install the Zotero Connector browser extension. Click the icon on any journal article page, library database, or Google Scholar result to save it directly to Zotero.
- **DOI/ISBN:** Click the magic wand icon in Zotero and paste a DOI or ISBN — Zotero fetches all metadata automatically.
- **Manual entry:** Click the green "+" button → select item type → fill in fields.
- **PDF import:** Drag a PDF into Zotero — it will attempt to extract metadata automatically.

## Step 4: Set Up Auto-Export

This is the key step that connects Zotero to Claude Code.

1. Right-click the **SALSU** collection (or whichever collection you want to export)
2. Select **Export Collection...**
3. Set format to: **Better CSL JSON**
4. Check **Keep updated** (this enables auto-export)
5. Save the file to:
   ```
   <your-project>/references/salsu-library.json
   ```
6. Click OK

The file will now update automatically whenever you add, edit, or remove items in that Zotero collection.

### Auto-Export Settings

To configure when the export updates:

1. In Zotero: Edit → Settings → Better BibTeX → Automatic export
2. Scheduling options:
   - **On change** — updates immediately when you modify the collection (recommended)
   - **When idle** — updates when Zotero has been idle for a few minutes
   - **Paused** — only updates manually (right-click export → Export now)

## Step 5: Download APA 7 CSL Style

The CSL (Citation Style Language) file tells Pandoc how to format citations in APA 7.

```bash
curl -L -o <your-project>/references/apa.csl \
  https://raw.githubusercontent.com/citation-style-language/styles/master/apa.csl
```

## Step 6: Install Pandoc (Optional)

Pandoc can render markdown with `[@citekey]` citations into formatted DOCX or PDF with proper APA 7 citations and a reference list.

```bash
brew install pandoc
```

### Test Pandoc rendering:

```bash
cd <your-project>
pandoc paper.md \
  --citeproc \
  --bibliography=references/salsu-library.json \
  --csl=references/apa.csl \
  -o paper.docx
```

## Step 7: Enable Local API (Optional)

If you want tools to query Zotero directly while it's running:

1. In Zotero: Edit → Settings → Advanced
2. Check: **Allow other applications on this computer to communicate with Zotero**
3. The API is now available at `http://127.0.0.1:23119`

Test with:
```bash
curl http://127.0.0.1:23119/connector/ping
```

This is not required for the citation-helper skill (which reads the exported JSON file), but enables future integrations like MCP servers.

## Verification

After setup, verify everything works:

1. **Zotero has items:** Your SALSU collection should contain at least a few references
2. **Export file exists:** Check that `references/salsu-library.json` exists and is valid JSON:
   ```bash
   python3 -c "import json; d=json.load(open('references/salsu-library.json')); print(f'{len(d)} items')"
   ```
3. **Citation keys are stable:** Open the JSON file and verify each item has a readable `id` field (the citation key)
4. **Auto-export works:** Add a test item to the SALSU collection in Zotero, wait a moment, then check if the JSON file updated

## Troubleshooting

| Problem | Solution |
|---------|----------|
| JSON file not updating | Check BBT auto-export settings: Edit → Settings → Better BibTeX → Automatic export. Ensure it's not "Paused". |
| Citation keys look wrong | Check BBT key format: Edit → Settings → Better BibTeX → Citation keys. Click "Refresh". |
| Zotero not saving from browser | Reinstall Zotero Connector extension. Ensure Zotero is running. |
| Duplicate citation keys | BBT adds suffixes automatically (`smith2024a`, `smith2024b`) for conflicts. |
| Large JSON file (>10MB) | Export only the relevant sub-collection instead of the entire library. |
