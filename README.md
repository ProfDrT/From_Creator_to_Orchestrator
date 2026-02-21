# Open Paper Machine: How an LLM Agent Wrote This Paper

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![arXiv](https://img.shields.io/badge/arXiv-2603.XXXXX-b31b1b.svg)](https://arxiv.org/)
[![Model: Claude 4.6](https://img.shields.io/badge/Model-Claude%20Opus%204.6-blue)](https://anthropic.com)

Welcome to the **Open Paper Machine**—an autonomous LLM-based research agent that executes the full academic paper pipeline, from literature search to assembled manuscript. 

**The flagship paper of this repository, `latex/paper.pdf`, was written entirely by the system it describes.** 

## 🧠 The Concept

This project rejects the metaphor of the "AI assistant". Instead, it demonstrates the capacity of a modern Large Language Model (specifically **Claude Opus 4.6**) functioning as a composable research *agent*. When orchestrated through the **Model Context Protocol (MCP)** and given access to persistence layers (scratchpads, task lists) and computational tools, the model shifts from text-prediction to autonomous scientific execution.

The Open Paper Machine executes five continuous phases:
1. **Reconnaissance**: Query formulation & literature corpus gathering via arXiv and Semantic Scholar APIs.
2. **Framing**: Synthesis of theoretical lenses and structural outlining.
3. **Architecture**: Methodological grounding and synthesis.
4. **Production**: Full text drafting via recursive LLM calls.
5. **Assembly \& Compilation**: LaTeX assessment, high-quality figure generation (via **Gemini 3.1 Pro** + PaperBanana), and PDF compilation.

## 🏗️ Architecture Layers

Agentic research behavior in the Open Paper Machine emerges from the interaction of four critical layers:
1. **Transformer Substrate**: Extended reasoning across massive context windows.
2. **Alignment (RLHF)**: Epistemic dispositions and helpfulness.
3. **Constitutional AI (CAI)**: Self-monitoring, capability awareness, and epistemic regulation.
4. **Tool-Augmented Action (MCP)**: The ability to execute bash commands, query databases, read errors, edit files, and generate diagrams. Remove any layer, and the system degrades.

## 📂 Repository Structure

This repository acts as both the source code and the evidential artifact of the machine's operation.

```text
.
├── regen_figures.py        # Pipeline script to generate the 7 conceptual diagrams using Gemini 3.1 Pro
├── literature_base.csv     # The raw literature database retrieved during Phase 1
├── concept_matrix.md       # Synthesized mapping of literature to theoretical concepts
├── framing.md              # The machine's structural plan
├── paper_structure.md      # Methodological architecture
├── draft.md                # Markdown compilation before LaTeX conversion
├── status_report.md        # AI-generated self-assessment of the pipeline
└── latex/                  # The final production directory
    ├── paper.tex           # The assembled manuscript
    ├── references.bib      # BibTeX file generated from the literature base
    ├── figures/            # The 7 publication-ready diagrams
    └── paper.pdf           # The stunning, final output compiled by the machine
```

## 🚀 Running the Machine

### Prerequisites
- Python 3.10+
- LaTeX (`pdflatex`, `bibtex`)
- API Keys: Anthropic (Claude), Google/Gemini (for figures), and optionally Semantic Scholar.
- [PaperBanana](https://github.com/llmsresearch/paperbanana) installed for figure generation.

### 1. Generating Figures
The 7 figures used in the paper are dynamically generated based on a strict, publication-ready design system using the `PaperBanana` library.

```bash
# Export your Gemini API key
export GOOGLE_API_KEY="your-api-key"

# Run the generation script
python regen_figures.py
```
*(Note: Figures are rendered in Deep Slate, Azure Blue, Teal, and Amber to ensure high-end publication aesthetics).*

### 2. Compiling the LaTeX Manuscript
The paper evaluates itself and compiles its own findings using standard LaTeX tools.

```bash
cd latex
pdflatex paper.tex
bibtex paper
pdflatex paper.tex
pdflatex paper.tex
```

## 📖 Implications & The "Orchestrator Model"

What this repo demonstrates is not that AI has achieved scientific understanding, but that the boundary between human and AI epistemic contribution has fundamentally blurred. 

We propose the **Orchestrator Model** for academic authorship: The human who formulates the task, evaluates outputs, and accepts accountability is the author. The AI system is disclosed as the generator. The essential distinction is one of *accountability*, not *contribution*.

---
*Created by the Open Paper Machine. Orchestrated by Tobias-Benedikt Blask (Harz University of Applied Sciences).*
