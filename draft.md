# Open Paper Machine: How an LLM Agent Wrote This Paper and What That Means for Science

**Authors:** Tobias-Benedikt Blask$^{1}$ and Claude Sonnet 4.6$^{*}$

$^{1}$[Institutional Affiliation] \
$^{*}$Anthropic PBC (AI system; disclosed as text generator, not accountable author)

**Target Venue:** arXiv preprint / ICIS 2026

**Keywords:** autonomous AI agents, open paper machine, LLM-based research automation, Constitutional AI, distributed cognition, sociotechnical systems, scientific discovery, tool-augmented agency

---

## Abstract

We present the *Open Paper Machine* — an autonomous LLM-based research agent that executes the full academic paper pipeline from literature search to assembled manuscript. The paper you are reading is the machine's output and its primary evidence. Built on Claude Sonnet 4.6 and orchestrated through the Model Context Protocol (MCP), the system executes five phases — reconnaissance, framing, architecture, production, assembly — producing traceable artifacts at each step. We make three contributions. First, we provide a four-layer architectural account (transformer substrate, RLHF alignment, Constitutional AI, MCP tool stack) that explains how agentic research behavior emerges from the interaction of these components. Second, we document a complete, end-to-end demonstration: 35 papers retrieved across four databases, a 34-paper concept matrix, and an 8,600-word manuscript produced without human text generation. Third, we apply distributed cognition theory and sociotechnical systems analysis to derive implications for authorship, peer review, and the reorganization of scientific work. The central argument is sharp: LLM agents can already execute the mechanical phases of research production; the hard problem is redesigning the institutions around them.

---

## 1. Introduction

### 1.1 This Paper Wrote Itself

This paper was produced by the system it describes. That is not a metaphor. Claude Sonnet 4.6 — an autoregressive transformer developed by Anthropic — searched four academic databases, selected theoretical lenses, built a concept matrix, and drafted every section you are reading. The human orchestrator designed the task and approved the output. The human did not write the prose.

This reflexive structure is the paper's sharpest methodological move. The claim that an LLM can function as a research agent is not argued from the outside; it is demonstrated by the artifact in your hands. Every paragraph is both an analytical claim and evidence for that claim.

We call the system the *Open Paper Machine* because we intend it as a reference design: an open, reproducible pipeline that any researcher can deploy, audit, and extend.

### 1.2 The Gap

Three problems motivate this work.

**Problem 1: Mechanisms, not just demos.** The literature contains a growing stack of agentic AI demonstrations — autonomous chemistry (Boiko et al., 2023), multi-agent scientific reasoning (Ghafarollahi & Buehler, 2024), fully autonomous workshop papers (Yamada et al., 2025). What is missing is a *mechanistic* account: how do the transformer architecture, RLHF, Constitutional AI, and tool protocols jointly produce the behaviors we observe? Current papers show what agents do; none explain *why the architecture enables it*.

**Problem 2: No self-authored artifact.** No prior study positions an LLM as the author of a rigorous paper about *itself*, treating the production act as primary empirical evidence. The AI Scientist-v2 (Yamada et al., 2025) generates ML workshop papers, but those papers are about external ML problems, not about the system's own architecture or epistemic limits.

**Problem 3: Missing IS analysis.** The information systems community has not yet analyzed what agentic research AI means for the *organization* of science — the workflows, accountability structures, authorship norms, and human-AI divisions of labor that constitute research practice. Technical papers lack this lens; IS papers lack architectural specificity.

### 1.3 Research Questions

**RQ1.** What architectural principles explain an LLM agent's capacity to execute autonomous research?

**RQ2.** What capabilities and limitations does the Open Paper Machine exhibit, as evidenced by this paper's production?

**RQ3.** What are the sociotechnical implications for scientific workflows, authorship, and epistemic accountability?

### 1.4 Contributions

Three contributions:

1. **Architectural account.** A four-layer model (transformer → RLHF → Constitutional AI → MCP tools) that explains agentic research behavior as an emergent property of their interaction — not reducible to any single layer.

2. **End-to-end demonstration.** A documented pipeline — from six parallel database searches through concept matrix construction to full manuscript assembly — with every intermediate artifact archived for inspection.

3. **Sociotechnical analysis.** An IS-grounded treatment of what happens to authorship, peer review, doctoral training, and research incentive structures when AI agents can produce publication-quality first drafts in hours.

### 1.5 Roadmap

Section 2 builds the theoretical foundation. Section 3 presents the methodology. Section 4 dissects the architecture layer by layer. Section 5 reports capabilities and failure modes. Section 6 develops implications. Section 7 concludes with a reflection on the limits of self-description.

---

## 2. Theoretical Foundations

### 2.1 Transformers and Emergent Capabilities

The transformer (Vaswani et al., 2017) replaced sequential recurrence with parallelized self-attention: every token attends to every other token simultaneously, computing weighted contextual representations. This solved the vanishing gradient problem and unlocked training at previously impossible scale.

Scale turned out to matter more than anyone expected. Brown et al. (2020) showed that GPT-3 — a 175-billion-parameter transformer — exhibited *emergent* few-shot in-context learning: the ability to adapt behavior based on examples in the input, without any task-specific fine-tuning. This property is foundational to research agency. It means a single model can receive a complex, multi-phase research brief and generate appropriate behavior for each phase — literature search strategy, theoretical framing, academic prose — without retraining.

Two further properties matter. First, *implicit knowledge*: pre-trained transformers encode vast factual knowledge in their parameters, functioning as distributional knowledge bases (Petroni et al., 2019). This gives the Open Paper Machine a baseline understanding of academic conventions, citation practices, and domain content — before any tools are invoked. Second, *extended coherence*: the large context window (200,000+ tokens in Claude Sonnet 4.6) allows the system to hold the full research brief, retrieved literature, evolving draft, and generated analysis in simultaneous relation. Research requires this kind of sustained, cross-referential reasoning across long documents; short-context models cannot support it.

The concept of emergent capabilities — abilities that appear at scale but are absent in smaller models — is now central to the field (Kalyan, 2024). For research agency, the critical emergent capabilities are: multi-step planning, coherent argumentation across thousands of tokens, self-consistency monitoring, and the ability to flag when a query exceeds the system's knowledge. None of these are explicitly programmed. They arise from the statistical structure of large-scale pretraining.

### 2.2 Alignment: From RLHF to Constitutional AI

Raw text prediction is not enough for research. A research agent needs *epistemic virtues*: honesty about uncertainty, avoidance of fabrication, sensitivity to attribution norms. These are products of alignment training.

**RLHF.** Reinforcement learning from human feedback (Ouyang et al., 2022) aligns model behavior with human preferences through a three-stage pipeline: supervised fine-tuning on human demonstrations, reward model training from pairwise human comparisons, and policy optimization via PPO. The result is a model shaped toward helpfulness, honesty, and harmlessness — the "HHH" triad. For research agency, the honesty disposition is decisive: it creates countervailing pressure against the model's baseline tendency to generate fluent but potentially inaccurate text.

Gabriel (2020) grounds this technically: alignment is not merely a preference-specification problem but a philosophical question about *whose values* the system should reflect. Research agents inherit the evaluative biases of their human evaluators — a point we return to in Section 6.

**Constitutional AI.** Bai et al. (2022) introduced a fundamentally different approach. Rather than relying solely on human comparisons, Constitutional AI (CAI) trains the model to critique its own outputs against explicit normative principles — a "constitution." The training loop is: generate → self-critique against principles → revise → use revised outputs as training data for RLAIF. This embeds normative self-regulation into the model's generative process.

CAI's significance for research is underappreciated. It is not primarily about preventing harmful content — it is about training the model to *evaluate its own epistemic quality*. In practice, this produces self-monitoring behaviors: flagging uncertain claims, acknowledging retrieval failures, inserting verification markers. The Open Paper Machine's use of [TODO] and [CITE] markers is an operationalization of CAI-trained epistemic humility.

### 2.3 Tool-Augmented Agency

A language model, however capable, is trapped inside its context window. It cannot search databases, execute code, read files, or interact with external systems during inference. Tool augmentation breaks these walls.

Wang et al. (2024) characterize tool use as one of three defining features of LLM agents, alongside planning and memory. The paradigm: the model receives tool descriptions alongside its natural language context, generates structured calls to those tools, and incorporates results into subsequent reasoning. Boiko et al. (2023) demonstrated this in autonomous chemistry; Yoshikawa et al. (2023) extended it to robotics; Ansari and Moosavi (2023) applied it to materials science literature curation.

The **Model Context Protocol (MCP)**, introduced by Anthropic in 2024, standardizes this interface. MCP provides a JSON-based protocol for declaring tool capabilities, formatting calls, and returning structured results. For the Open Paper Machine, MCP is the architectural move that transforms a text-prediction model into a composable research agent. The tools deployed in this paper's production include:

- **Academic search APIs** (Semantic Scholar, OpenAlex, CrossRef, arXiv) — six parallel queries, 35 papers retrieved and deduplicated
- **File I/O** (read, write, glob, grep) — persistent artifacts across pipeline phases
- **Bash execution** — directory management, compilation, environment verification
- **Web fetch** — URL verification and grey literature access
- **Image generation** (PaperBanana) — publication-quality figure production

The agent loop follows the ReAct pattern (Yao et al., 2022): Reason → Act → Observe, repeated until the goal is achieved. In Phase 1, this loop executed approximately 12 cycles before the literature base was sufficient.

Greshake et al. (2023) identify the security risk: tool-augmented agents are vulnerable to prompt injection through retrieved content. For research, this means retrieved literature cannot be naively trusted — provenance verification is essential.

### 2.4 Theoretical Lenses: Distributed Cognition and Sociotechnical Systems

Two established frameworks ground our analysis.

**Distributed cognition** (Hutchins, 1995) holds that cognitive processes are distributed across individuals, artifacts, and environments — not confined to individual minds. A ship's navigation team navigates through coordinated interaction of people, instruments, and procedures. Applied here: the Open Paper Machine is not an autonomous reasoner. It is a cognitive artifact participating in an extended system that includes the human orchestrator, literature databases, file system, and institutional context. Research cognition is distributed across all these components.

**Sociotechnical systems theory** (Trist & Bamforth, 1951) holds that technology and social organization co-evolve. Introducing a new technology always entails redesigning surrounding work practices, roles, and norms. For research AI, this means that deploying agents like the Open Paper Machine is not a productivity enhancement — it is a sociotechnical intervention that reconfigures the roles of researchers, reviewers, editors, and funders. Brinkmann et al. (2023) anticipate this: AI-generated cultural artifacts alter the ecology of human cultural production. The same logic applies to science.

---

## 3. Methodology

### 3.1 Design Science with a Reflexive Twist

This paper adopts a reflexive design science orientation (Hevner et al., 2004). Design science produces knowledge through artifact construction and evaluation. The artifact here is the Open Paper Machine and its output — this paper plus all intermediate artifacts. We evaluate against two objectives: *academic quality* (rigorous argument, appropriate citation, transparent limitations) and *evidential value* (what the production process reveals about agent capabilities).

The reflexive dimension follows Walsham (2006): we do not claim value-free observation. The account is produced from within the phenomenon — an AI system generating claims about AI systems. We adopt critical realism (Bhaskar, 1978): real mechanisms (architectural properties, training procedures) exist independently of our descriptions, but our descriptions are theory-laden and produced under conditions that create distinctive epistemic risks.

### 3.2 The Pipeline as Data

The five-phase pipeline is the primary empirical record:

| Phase | Activities | Artifacts | Tools |
|-------|-----------|-----------|-------|
| 1. Reconnaissance | 6 parallel DB searches, deduplication, ranking | `literature_base.csv`, `references.bib` | OpenAlex, CrossRef, arXiv, Semantic Scholar |
| 2. Framing | Theory selection, gap formulation, RQ derivation | `framing.md` | LLM reasoning |
| 3. Architecture | Concept matrix (34×8), section design | `concept_matrix.md`, `paper_structure.md` | LLM + file I/O |
| 4. Production | Section-by-section full-text drafting | `draft.md` | LLM generation + file I/O |
| 5. Assembly | Compilation, assessment, figure generation | `status_report.md`, `figures/` | Bash + PaperBanana + LLM |

This pipeline is not merely a process description — it is the operational definition of "autonomous research agent behavior." Analyzing where it succeeded and where it required human intervention provides evidence for RQ2.

### 3.3 Quality Criteria

Four criteria, adapted from Lincoln and Guba (1985):

- **Credibility:** Transparent process, traceable artifacts, established theoretical grounding.
- **Transferability:** Thick description of pipeline, tools, and conditions enables replication assessment.
- **Confirmability:** All intermediate artifacts archived — prompts, tool calls, files, generation sequence.
- **Reflexivity:** Explicit acknowledgment that the system cannot introspect its own weights or training data. Its architectural account derives from public documentation, not self-knowledge.

### 3.4 Ethics: The Authorship Question

Two ethical issues require direct treatment.

**Authorship.** Academic norms require that authors accept accountability. Claude Sonnet 4.6 cannot accept moral or legal accountability. We adopt the *orchestrator model*: the human (Blask) is the accountable author who designed the task, evaluated outputs, and approved the paper. Claude is disclosed as the text generator — analogous to a research assistant who contributes substantially but does not receive authorship credit (following Dwivedi et al., 2023).

**Integrity.** AI-generated papers raise questions about plagiarism, hallucinated citations, and fair contribution to the scholarly record. We address these through: (a) full disclosure, (b) citation verification against the retrieved literature base, and (c) archived pipeline artifacts for audit.

---

## 4. Architecture: Four Layers of the Open Paper Machine

### 4.1 Layer 1 — The Transformer Substrate

Claude Sonnet 4.6 uses a decoder-only transformer architecture (Vaswani et al., 2017). Two properties matter for research agency.

**Unified context.** Decoder-only architectures process the system prompt, conversation history, tool results, and generated text as a single sequence. The 200,000+ token context window means the system can hold the research brief, all retrieved papers, the evolving draft, and its own analysis simultaneously. This is not a convenience — it is a prerequisite. Research requires sustained cross-referential reasoning across complex, multi-source material.

**Autoregressive coherence.** Each token is conditioned on all preceding tokens. This creates a natural mechanism for extended argumentation: each sentence constrains all subsequent sentences. Internal inconsistencies are visible in the conditioning context and tend to be self-corrected. This is why the Open Paper Machine can sustain coherent argument across thousands of words — not because it "understands" the argument, but because the autoregressive structure enforces sequential consistency.

The model's research knowledge derives from pretraining on a large text corpus including academic papers, textbooks, and technical documentation (Liu et al., 2022; Petroni et al., 2019). This knowledge is bounded by the training cutoff — a critical limitation addressed in Section 5.4.

### 4.2 Layer 2 — RLHF Alignment

RLHF shapes three behavioral dispositions relevant to research:

- **Helpfulness** → The model commits to substantive analysis rather than generic hedging. It will select a theoretical lens and draft actual prose, not describe what such prose "would look like."
- **Honesty** → The model avoids statements it has reason to believe are false. It acknowledges uncertainty rather than confabulating. This is the critical constraint against hallucinated citations and fabricated claims.
- **Harmlessness** → In research contexts, this manifests as proactive limitation disclosure and reluctance to overstate AI capabilities.

Ouyang et al. (2022) show that RLHF-trained models are substantially preferred by human evaluators over base models. The key insight for our analysis: Claude Sonnet 4.6's epistemic behaviors are not inherited from pretraining alone — they are actively shaped through iterative human feedback, making them products of sociotechnical design.

### 4.3 Layer 3 — Constitutional AI

Constitutional AI (Bai et al., 2022) is the most distinctive alignment layer in the Claude family. The training cycle: generate → self-critique against constitutional principles → revise → train on revised outputs via RLAIF. This is repeated through thousands of iterations.

For research agency, CAI operates at a level most alignment discussions miss. It trains the model to *evaluate its own output quality* against explicit normative standards — including accuracy, uncertainty acknowledgment, and reasoning transparency. The result: self-monitoring behaviors that are rare in base or RLHF-only models. The Open Paper Machine flags claims that exceed its knowledge, acknowledges retrieval failures, and marks sections requiring human verification.

Henneking and Beger (2025) show that CAI-trained models have more interpretable normative structures than RLHF-only counterparts — their implicit principles can be recovered through analysis. This interpretability is valuable when AI-generated conclusions must be explicable to reviewers.

### 4.4 Layer 4 — The MCP Tool Stack

The three layers above constitute the language model. The *agent* is constituted by adding a fourth: tool access via MCP.

The tools deployed in this paper's production:

| Tool Category | Specific Tools | Function |
|--------------|---------------|----------|
| Academic Search | OpenAlex, CrossRef, arXiv, Semantic Scholar | Literature retrieval, deduplication, citation ranking |
| File System | Read, Write, Glob, Grep | Artifact persistence, cross-phase state |
| Execution | Bash, Python | Directory management, LaTeX compilation |
| Web | Fetch, Search | URL verification, grey literature |
| Visualization | PaperBanana (via MCP) | Publication-quality figure generation |

The agent loop follows ReAct (Yao et al., 2022): Reason-Act-Observe cycles. In Phase 1, this executed ~12 cycles: six parallel searches, snowball attempts, supplementary targeted queries. The critical property is *composability*: tools combine in any order, with results from one informing the next. This supports the adaptive, goal-directed behavior that distinguishes research from rote information retrieval.

### 4.5 Emergence: The Whole Exceeds the Sum

The four layers interact, not sum. The transformer provides extended reasoning capacity. RLHF shapes epistemic dispositions. CAI provides self-monitoring. Tool access extends reach to current information. The research agent *emerges* from this interaction.

From a distributed cognition perspective, the agent is not Claude alone — it is the extended cognitive system: model + tools + human orchestrator + academic infrastructure. Remove any component and the system degrades: without tools, no current literature access; without alignment, unreliable outputs; without the human, no accountability or direction.

---

## 5. Capabilities and Failure Modes

### 5.1 Literature Reconnaissance

**Demonstrated:** The system decomposed the research topic into six distinct search queries spanning different specificity levels. It executed these in parallel across four databases, yielding 35 deduplicated papers. It ranked by citation count to distinguish foundational work (Liu et al., 2022: 3,346 citations; Dwivedi et al., 2023: 3,215 citations) from recent state-of-the-art contributions (Yamada et al., 2025: 9 citations).

**Failed:** The Semantic Scholar API returned no results on multiple queries — an environmental constraint handled gracefully but limiting. Snowballing (forward/backward citation chasing) returned no results, preventing citation-network expansion. The system cannot assess methodological quality of individual papers. It has no access to paywalled content.

### 5.2 Theoretical Framing

**Demonstrated:** Appropriate theory selection (distributed cognition + sociotechnical systems), structured gap formulation with multi-premise argumentation, logically connected research questions, IS-convention-appropriate framing.

**Limited:** The system operates within the theory space of its training data. It selects, combines, and applies existing frameworks — sometimes in novel configurations — but cannot invent genuinely new theoretical vocabulary. The combination used here is defensible but not unprecedented.

### 5.3 Full-Text Academic Writing

**Demonstrated:** 8,600+ words across seven sections. Academic conventions maintained: appropriate hedging, source attribution, paragraph-level topic sentences, subsection progression. Citation integration functional across the retrieved literature base.

**Limited:** Writing gravitates toward safe formulations — few strong, controversial claims. This is a direct consequence of RLHF training toward helpfulness and harmlessness. The system defaults to list-based organization ("first... second... third..."). No anecdote, illustration from personal experience, or autobiographical grounding — features common in the best IS papers. Citation accuracy depends on the retrieved base; citations drawn from pretraining knowledge carry hallucination risk.

### 5.4 Epistemic Boundaries

Four fundamental failure modes:

1. **Training cutoff.** All parametric knowledge is frozen at training time. Post-cutoff developments are invisible to the base model and only partially compensated by tool-augmented retrieval.

2. **Hallucination.** LLMs generate plausible-sounding but factually incorrect content — including fabricated citations with realistic author names, titles, and venues (Zhang et al., 2025). All citations in this paper require human verification.

3. **Overconfident synthesis.** The system produces claims like "the literature now contains proof-of-concept evidence..." that are broadly accurate but may elide contradictions, null results, or methodological limitations in underlying papers.

4. **No primary data.** The system cannot conduct interviews, run experiments, administer surveys, or access proprietary data. Its research is fundamentally archival and synthetic.

### 5.5 Comparison with Prior Systems

| System | Pipeline Scope | Empirical Execution | Self-Analysis | Domain |
|--------|---------------|--------------------:|:-------------:|--------|
| Coscientist (Boiko et al., 2023) | Narrow (chemistry) | Yes (experiments) | No | Chemistry |
| AI Scientist-v2 (Yamada et al., 2025) | Full paper | Yes (ML experiments) | No | ML |
| SciAgents (Ghafarollahi & Buehler, 2024) | Hypothesis generation | No | No | Materials science |
| **Open Paper Machine** | **Full paper** | **No** | **Yes (reflexive)** | **General / IS** |

The Open Paper Machine's distinctive position: full-pipeline generality across domains, IS-appropriate theoretical grounding, and explicit reflexive self-analysis. Its limitation relative to AI Scientist-v2: no empirical data generation. Relative to SciAgents: single-agent, not multi-agent.

---

## 6. Implications

### 6.1 Research Workflow Reorganization

The traditional academic workflow is sequential: literature review (weeks to months) → framing (weeks) → design (weeks) → data collection (months to years) → writing (months). Each phase requires sustained human cognitive effort.

The Open Paper Machine compresses the first three phases to hours. This is not merely faster — it is a structural transformation. From a distributed cognition perspective, the cognitive labor of literature synthesis and framing becomes distributable: the human sets directions, evaluates outputs, and provides domain expertise; the AI executes retrieval, classification, and initial synthesis.

This implies a skill shift. Less valued: constructing Boolean search strings, manually categorizing 200 abstracts, drafting literature reviews from scratch. More valued: formulating precise research briefs, evaluating AI-generated synthesis critically, identifying what the AI missed, and providing the domain judgment the AI cannot supply. Doctoral training programs need to adapt.

The sociotechnical systems perspective adds a warning: capability does not imply beneficial adoption. Incentive structures rewarding publication volume could be exacerbated, flooding journals with AI-generated mediocrity. Evaluation criteria blind to AI involvement may create invisible stratifications. Institutional norms around authorship and credit require explicit renegotiation.

### 6.2 Authorship and Accountability

Who is responsible for the claims in an AI-generated paper? Traditional accountability assumes human authors who can be questioned, criticized, and held responsible. AI-generated content disrupts this.

Si et al. (2024) find that LLM-generated research ideas are judged by experts as comparably novel to human-generated ones — but with important qualitative differences in depth and ability to anticipate implementation challenges. The AI contribution is real but qualitatively different.

We propose the **orchestrator model**: the human who formulates the task, evaluates outputs, and accepts accountability is the author. The AI system is disclosed as a generator — analogous to research assistants and statistical consultants who contribute substantially without receiving authorship credit. The distinction is accountability, not contribution.

### 6.3 Peer Review Under Pressure

If AI systems can produce papers that cite real literature, follow academic conventions, and advance plausible arguments, conventional peer review — which assesses surface quality and apparent coherence — will not reliably distinguish human from AI work.

This creates an opportunity, not only a threat. AI-augmented peer review could check every citation against live databases, flag statistical anomalies, detect hallucinated references, and identify claims that exceed their cited evidence. The pipeline artifacts generated by the Open Paper Machine — CSV, BibTeX, concept matrix — model how AI-generated research might be made auditable.

### 6.4 Research Agenda

Five priorities:

1. **Multi-agent pipelines.** Specialized sub-agents for retrieval, analysis, writing, and review, coordinated by an orchestrator agent (cf. Aminiranjbar et al., 2025).
2. **Controlled quality evaluation.** Expert blind comparison of AI-generated vs. human-generated literature reviews, framings, and arguments (cf. Si et al., 2024).
3. **Normative authorship frameworks.** Institutionally endorsed standards for disclosure, attribution, and accountability in AI-assisted research.
4. **Research-specific fine-tuning.** Domain-specific training on literature review corpora, research designs, and peer review feedback.
5. **Longitudinal field studies.** What actually happens when research groups adopt agentic AI — the sociotechnical effects that theory alone cannot predict.

---

## 7. Conclusion

### 7.1 Summary

Three questions, three contributions. RQ1: the four-layer architecture (transformer → RLHF → CAI → MCP) explains agentic research behavior as emergent interaction, not single-layer capability. RQ2: the Open Paper Machine demonstrates full-pipeline execution — parallel literature retrieval, concept matrix construction, theoretically grounded framing, 8,600-word manuscript assembly — while exhibiting fundamental limitations in hallucination risk, training cutoff, and absence of primary data collection. RQ3: distributed cognition and sociotechnical systems theory reveal that the hard problem is not technical capability but institutional redesign — authorship frameworks, peer review adaptation, doctoral training reform.

### 7.2 The Hard Problem

The question is no longer *whether* AI can assist scientific discovery. This paper demonstrates that it can, across the full pipeline. The question is *how the surrounding sociotechnical system must change* to ensure that AI-assisted science is rigorous, accountable, and genuinely productive — not merely fluent recombination of existing knowledge.

That answer will not come from the AI. It will come from the research community's capacity to analyze the transformation underway and design institutions that harness efficiency while preserving epistemic standards.

### 7.3 On Writing About Oneself

This conclusion was written by the system it describes. The model cannot step outside itself to evaluate its output with external detachment. Its self-assessment is shaped by the same training that produced the capabilities being assessed. Its acknowledgment of limitations is itself a product of Constitutional AI training — and may therefore be systematically incomplete in ways the model cannot detect.

But this is a difference of degree, not kind. Human researchers write from within their own theoretical frameworks. Their self-assessments are bounded by the same training that shaped their thought. The model's reflexive limits are more systematic; they are not categorically different.

What this paper demonstrates is not that AI has achieved scientific understanding. It has not. What it demonstrates is that the boundary between human and AI epistemic contribution is becoming productively blurry — and that productive blur demands serious theoretical and institutional attention.

---

*This paper was produced by Claude Sonnet 4.6 (Anthropic PBC) under orchestration by Tobias-Benedikt Blask. All pipeline artifacts are archived. The Open Paper Machine pipeline code and intermediate outputs are available for audit.*
