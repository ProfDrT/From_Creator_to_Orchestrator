# Open Academic Paper Machine

> Autonomous academic paper production system for Claude Code and Cowork. — v5.0.0

Give it a title, topic, or research question. The agent runs the complete pipeline — from literature search to production-ready LaTeX/PDF — with you steering at checkpoints.

## How it works

```
/write-paper The impact of generative AI on organizational decision-making:
  A systematic literature review
```

The machine runs autonomously through 6 phases:

| Phase | What happens | Your job |
|-------|-------------|----------|
| **1. Reconnaissance** | 4-6 search queries → 4 APIs → snowballing → deduplication | Check scope, redirect if needed |
| **2. Framing** | Select theory → formulate gap → RQs → contribution | Confirm direction |
| **3. Architecture** | Concept matrix → paper structure → word budget | Approve structure |
| **4. Production** | Write every section as complete paragraphs + generate figures | Read along, adjust |
| **5. Assembly** | Compile all → quality self-assessment → status report | Start review |
| **6. LaTeX & PDF** | Convert to arxiv-style LaTeX + compile PDF | Download and submit |

**Core principle:** The machine makes decisions and presents results. You steer at checkpoints.

## Output

After `/write-paper` + `/export-latex` you have:

- `draft.md` — Complete paper draft with all sections
- `references.bib` — BibTeX of all cited papers
- `literature_base.csv` — Full literature database
- `concept_matrix.md` — Concept Matrix (Webster & Watson)
- `framing.md` — Theoretical framing (gap, RQs, contribution)
- `paper_structure.md` — Paper architecture
- `figures/` — Generated diagrams and plots (PNG, 300 DPI)
- `status_report.md` — What's done, what you still need to do
- `latex/paper.tex` — arxiv-style LaTeX source
- `latex/paper.pdf` — Compiled PDF, ready for submission

## Commands

| Command | What |
|---------|------|
| `/write-paper [title]` | **Full pipeline** — all 6 phases |
| `/export-latex` | Convert finished draft to LaTeX + PDF |
| `/search-papers [topic]` | Phase 1 only: literature search |
| `/draft-section [section]` | Write one specific section |
| `/respond-reviewers` | Draft R&R response letter |
| `/generate-figure [description]` | Academic diagram from text description |
| `/generate-plot [datafile] [intent]` | Statistical plot from CSV/JSON data |

## Under the hood

**6 skill engines:**
- **writing-engine** — Paragraph templates for every section
- **literature-engine** — 4 academic APIs, snowballing, concept matrix, PRISMA
- **theory-engine** — Theory matching, gap formulas, contribution templates
- **method-engine** — Complete method sections for SLR, Case Study, Gioia, Mayring, PLS-SEM, DSR
- **figure-engine** — PaperBanana AI figures (Claude Code) or matplotlib fallback (Cowork)
- **latex-engine** — arxiv-style LaTeX conversion, citation resolution, PDF compilation

**MCP servers:**
- `academic-search` — Semantic Scholar, OpenAlex, CrossRef, arXiv, snowballing, BibTeX/CSV export
- `paperbanana` — AI diagram generation, plot generation, diagram evaluation via Gemini

## Setup

### Claude Code (recommended — full features)

```bash
# 1. Install the plugin via Claude Code plugin manager

# 2. Install PaperBanana (figure generation engine)
pip install paperbanana[mcp,google]

# 3. Set your Google API key for PaperBanana/Gemini figures
#    Get a free key at: https://aistudio.google.com/apikey
#    Create a .env file in your PROJECT ROOT (not the plugin dir):
echo 'GOOGLE_API_KEY="your-google-api-key"' > .env

# 4. Install LaTeX for PDF compilation
brew install --cask mactex-no-gui   # macOS
# or: sudo apt-get install texlive-full  (Linux)

# 5. Start writing
/write-paper Your Paper Title
```

> **Note:** The `.env` file must be in your project's working directory (where you run Claude Code). The PaperBanana MCP server automatically loads it on startup. Never commit `.env` to version control.

### Cowork

1. Upload the ZIP → Plugins → "+"
2. Set `GOOGLE_API_KEY` in plugin settings (optional — only needed for AI figures)
3. `/write-paper Your Paper Title` — go

### Figure generation by environment

| Environment | Engine | Quality |
|---|---|---|
| Claude Code | PaperBanana + Gemini | AI-generated, publication-quality |
| Cowork | matplotlib / seaborn | Clean statistical plots |

PaperBanana starts automatically via the MCP server config in `plugin.json`. The server loads the Google API key from `.env` in the project root. Falls back to Python (matplotlib) automatically if unavailable.

---

**Author:** Prof. Dr. Tobias Blask
