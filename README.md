# Scholar — Academic Writing Plugin for Claude Code

A complete academic writing pipeline: draft essays, proposals, reports, and systematic reviews with APA 7 citations, then audit, fix, and grade — all in one plugin.

## Skills

| Skill | Invoke | Purpose |
|-------|--------|---------|
| **research-proposal** | `/scholar:research-proposal` | Structured research proposals (2,000 words, SDG-linked, .docx output) |
| **academic-essay** | `/scholar:academic-essay` | Essays, literature reviews, scoping reviews, PRISMA-compliant systematic reviews |
| **project-report** | `/scholar:project-report` | Project reports with BMC, concept notes, theory integration |
| **systematic-review** | `/scholar:systematic-review` | 7-phase SLR pipeline: question → search → screen → extract → assess → synthesise → manuscript |
| **academic-editor** | `/scholar:academic-editor` | 5-pass editorial review (structural, copy, voice, citation, rubric) |
| **proposal-grader** | `/scholar:proposal-grader` | Grade proposals against rubric, predict A-F, revision priorities |
| **citation-helper** | `/scholar:citation-helper` | Search Zotero library, get [@citekey], format APA 7 entries |
| **citation-checker** | `/scholar:citation-checker` | 5-agent citation audit: cross-refs, APA 7, DOI validation, claim grounding |
| **citation-fixer** | `/scholar:citation-fixer` | 3-agent correction: metadata fixes, format fixes, gap markers |

## Workflow Chains

Skills chain together for end-to-end academic work:

```
DRAFT  →  research-proposal / academic-essay / project-report / systematic-review
REVIEW →  academic-editor (5-pass) + proposal-grader
AUDIT  →  citation-checker (5 parallel verification agents)
FIX    →  citation-fixer (3 parallel correction agents)
```

## Prerequisites

| Tool | Required | Used by |
|------|----------|---------|
| **Zotero 7** + Better BibTeX | Yes (for citation skills) | citation-helper, citation-checker, citation-fixer |
| **Node.js** + `docx` package | Yes (for .docx output) | research-proposal, academic-essay, project-report, systematic-review |
| **Pandoc** | Optional (for citeproc rendering) | citation-helper, systematic-review |
| **Python 3** + `openpyxl` | Optional (for .xlsx output) | systematic-review |

### Install prerequisites

```bash
# Node.js docx library (for Word document generation)
npm install -g docx

# Pandoc (optional, for automatic citation rendering)
brew install pandoc

# Python openpyxl (optional, for review matrix spreadsheets)
pip3 install openpyxl
```

## Installation

### From GitHub (recommended)

```bash
claude plugin install --from github:mcmespinaa/scholar-support
```

Or clone manually:

```bash
git clone https://github.com/mcmespinaa/scholar-support.git ~/.claude/scholar
```

### Claude Code CLI (local)

```bash
claude plugin install ./scholar
```

### Claude Desktop (Cowork)

1. Package: `cd scholar && zip -r ../scholar.zip . -x "*.DS_Store" -x ".git/*" && cd ..`
2. Open Claude Desktop > Cowork tab
3. Click Customize > Browse plugins > Upload a custom plugin file
4. Select `scholar.zip` > Install

### First time? New to all of this?

See **[SETUP.md](SETUP.md)** for a complete step-by-step guide that takes you from zero (no VS Code, no Node.js, nothing) to a fully working setup. Works on macOS, Windows, and Linux.

## Setup

### 1. Configure your Zotero library

The citation skills need a CSL-JSON export of your Zotero library:

1. Install [Zotero 7](https://www.zotero.org/download/) and [Better BibTeX](https://github.com/retorquere/zotero-better-bibtex/releases/latest)
2. Create a collection in Zotero for your research
3. Right-click the collection > Export > Better CSL JSON > check "Keep updated"
4. Save to a known path (e.g., `references/salsu-library.json`)

See `skills/citation-helper/references/zotero-setup.md` for the full guide.

### 2. Create your local configuration

Copy the template and update paths:

```bash
cp templates/local.md .local.md
```

Edit `.local.md` to set:
- `ZOTERO_LIBRARY` — path to your CSL-JSON export
- `CSL_FILE` — path to your APA 7 CSL file
- `LOCAL_PDFS` — path to your downloaded academic PDFs

### 3. (Optional) Download APA 7 CSL

If you want Pandoc to render citations automatically:

```bash
curl -o references/apa7.csl https://raw.githubusercontent.com/citation-style-language/styles/master/apa.csl
```

## Usage Examples

```
# Write a research proposal
/scholar:research-proposal "SDG 12 and circular economy leadership in Swedish SMEs"

# Write a systematic literature review
/scholar:systematic-review "digital transformation in social enterprises" phase:1

# Edit a draft before submission
/scholar:academic-editor "drafts/my_essay_draft.md"

# Check all citations in a document
/scholar:citation-checker "drafts/my_essay_draft.md"

# Fix citation issues automatically
/scholar:citation-fixer "drafts/my_essay_draft.md"

# Grade a research proposal
/scholar:proposal-grader "drafts/research_proposal_v2.md"

# Find references on a topic
/scholar:citation-helper "transformative social innovation"

# Write a project report with BMC
/scholar:project-report "sustainable food cooperative" framework:BMC
```

## Plugin Structure

```
scholar/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── research-proposal/    (SKILL.md + references/)
│   ├── academic-essay/       (SKILL.md + references/)
│   ├── project-report/       (SKILL.md + references/)
│   ├── systematic-review/    (SKILL.md + references/)
│   ├── academic-editor/      (SKILL.md + references/)
│   ├── proposal-grader/      (SKILL.md + references/)
│   ├── citation-helper/      (SKILL.md + references/)
│   ├── citation-checker/     (SKILL.md + references/)
│   └── citation-fixer/       (SKILL.md + references/)
├── references/
│   └── apa7-guide.md         (shared APA 7 formatting rules)
├── templates/
│   └── local.md              (configuration template)
├── settings.json
├── .gitignore
├── LICENSE
└── README.md
```

## License

MIT
