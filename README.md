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

## Key Features

- **9 skills** covering the full academic writing lifecycle
- **Configurable Zotero integration** — connects to any Zotero library via CSL-JSON export
- **4-tier source lookup** — Zotero → Local PDFs → NotebookLM → Web (graceful degradation if any tier is unavailable)
- **Cross-skill chaining** — skills reference each other and hand off results automatically
- **APA 7 throughout** — consistent citation style across all skills
- **Dual .docx output** — Pandoc (primary, with automatic citation rendering) or docx-js (fallback, fine-grained control)
- **Adaptable rubrics** — proposal-grader uses Hug & Aeschbach by default, accepts custom rubrics
- **Voice profiles** — academic-editor maintains author voice consistency via configurable profiles
- **Report merging** — citation-checker's 5 agents produce a unified report with defined ownership, severity mapping, and deduplication rules

## Prerequisites

| Tool | Required | Used by |
|------|----------|---------|
| **Zotero 7** + Better BibTeX | Yes (for citation skills) | citation-helper, citation-checker, citation-fixer |
| **Node.js** + `docx` package | Yes (for .docx output) | research-proposal, academic-essay, project-report, systematic-review |
| **Pandoc** | Recommended (primary .docx pathway) | citation-helper, systematic-review |
| **Python 3** + `openpyxl` | Optional (for .xlsx output) | systematic-review |

### Install prerequisites

```bash
# Node.js docx library (for Word document generation)
npm install -g docx

# Pandoc (recommended, for automatic citation rendering)
brew install pandoc          # macOS
# sudo apt install pandoc    # Linux
# winget install JohnMacFarlane.Pandoc  # Windows

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
4. Save to a known path (e.g., `references/my-library.json`)

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

## Edge Cases and Limitations

- **No reference list in document?** Citation-checker switches to reduced-scope mode (completeness check only)
- **.docx input?** Converted to markdown via Pandoc before processing. If Pandoc unavailable, export as .md/.txt first
- **Zotero not configured?** Citation skills skip Zotero lookup and fall back to local PDFs → web search
- **Non-APA citation style?** Cross-reference and grounding checks still work, but formatting validation is APA 7 only
- **Systematic review with few studies?** The pipeline adapts — narrative synthesis works with any count, but flags thin evidence

## Plugin Structure

```
scholar/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── research-proposal/    (SKILL.md + references/grading-rubric.md)
│   ├── academic-essay/       (SKILL.md)
│   ├── project-report/       (SKILL.md + references/theory-frameworks.md)
│   ├── systematic-review/    (SKILL.md + references/search-strategy-templates.md)
│   ├── academic-editor/      (SKILL.md + references/voice-profiles/)
│   ├── proposal-grader/      (SKILL.md + references/grading-rubric.md)
│   ├── citation-helper/      (SKILL.md + references/zotero-setup.md, csl-json-schema.md)
│   ├── citation-checker/     (SKILL.md + references/verification-patterns.md)
│   └── citation-fixer/       (SKILL.md + references/correction-rules.md)
├── references/
│   ├── apa7-guide.md         (shared APA 7 formatting rules)
│   ├── prisma-checklist.md   (shared PRISMA 2020 checklist)
│   └── review-methodology.md (shared SLR methodology guide)
├── templates/
│   └── local.md              (configuration template — copy to .local.md)
├── SETUP.md                  (zero-to-working guide for macOS, Windows, Linux)
├── settings.json
├── .gitignore
├── LICENSE (MIT)
└── README.md
```

## Changelog

### v1.0.0 (2026-03-31)

- Initial release: 9 skills bundled
- Cross-platform SETUP.md (macOS, Windows, Linux)
- Windows-specific guidance from real user testing (Git Bash, PowerShell execution policy, PATH issues)
- Semantic QA pass on citation-checker and systematic-review:
  - Citation-checker: Report Merging Rules (agent ownership, severity mapping, confidence→symbol, health %, dedup)
  - Citation-checker: edge case handling (no reference list, .docx parsing, Zotero graceful degradation)
  - Systematic-review: Pandoc as primary .docx pathway, docx-js as fallback
  - Systematic-review: explicit Phase 2→3 handoff with user instructions
  - Systematic-review: Phase 5→6 quality score consumption rules
  - Systematic-review: PCC question template alongside PICO

## License

MIT
