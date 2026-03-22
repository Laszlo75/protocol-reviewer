# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Claude Code skill (custom prompt) that reviews clinical protocols against current national guidelines and published evidence. When activated, it reads an uploaded clinical protocol (PDF/Word), searches PubMed and Scholar Gateway for recent evidence, cross-references with UK national guidelines (BTS, NICE, KDIGO, etc.), and produces a structured .docx review document with actionable recommendations.

## Repository Structure

- **SKILL.md** — The skill definition (prompt). This is the main file. It defines the 8-step workflow: read protocol → identify domain → search guidelines → search evidence → cross-reference → generate review → verify references → log to evaluation register.
- **assets/reference.docx** — Pandoc reference template for .docx output (Arial, A4, navy headings, headers/footers).
- **references/document_template.md** — Markdown template and pandoc conversion instructions for the review document.
- **references/pubmed_strategy.md** — PubMed search construction patterns and quality assessment heuristics.
- **evals/evals.json** — Test scenarios for the skill (ABOi transplant, perioperative anticoagulation, CMV prophylaxis).
- **reviews/** — Local evaluation register (gitignored). Contains `evaluation_register.csv` for tracking review outcomes.

## Key Design Decisions

- **Markdown-first approach**: The review is written as Markdown with YAML frontmatter, then converted to .docx via pandoc. This keeps the agent focused on clinical content rather than docx formatting.
- **Reference ledger**: During search, references are recorded in a structured ledger (PMID, DOI, authors, title copied verbatim from PubMed). The .bib and reference list are generated solely from this ledger to prevent DOI/title fabrication.
- **Dual search strategy**: Both PubMed MCP (keyword/MeSH) and Scholar Gateway (semantic search) are used for each topic — they return complementary results.

## Build / Conversion Command

```bash
pandoc "[Protocol_Name]_Review_[Year].md" \
  -o "[Protocol_Name]_Review_[Year].docx" \
  --reference-doc=assets/reference.docx \
  --from=markdown+yaml_metadata_block \
  --to=docx
```

## Output Files (per review)

1. `[Protocol_Name]_Review_[Year].md` — Markdown source
2. `[Protocol_Name]_Review_[Year].docx` — Word document
3. `[Protocol_Name]_References.bib` — BibTeX for Zotero
4. `[Protocol_Name]_PMIDs.txt` — PMID list for Zotero bulk import

## Tool Dependencies

- **PubMed MCP** (`search_articles`, `get_article_metadata`, `get_full_text_article`, `find_related_articles`)
- **Scholar Gateway** (`semanticSearch`)
- **WebSearch / WebFetch** — for national guidelines
- **pandoc** — markdown to .docx conversion

## AI Use Policy (ISO 42001)

**System identity:** Claude Opus 4.6 (Anthropic), accessed via Claude Desktop with PubMed and Scholar Gateway integrations. This skill requires Opus 4.6 for the clinical reasoning and evidence synthesis quality needed.

**Intended use:** AI-assisted evidence synthesis to support the review of clinical protocols against current national guidelines and published literature. The system retrieves, structures, and summarises evidence; it does not make clinical decisions.

**Human oversight:** All AI-generated outputs are advisory only. Every review must be critically appraised by a consultant-level clinician before informing protocol changes. Final recommendations are the responsibility of the reviewing clinician and the approving MDT.

**Transparency:** Each review document is generated as an explicit draft with a prominent "DRAFT — NOT FOR CLINICAL USE" callout. The transparency disclaimer (section 6) discloses AI involvement and includes a "Reviewed and approved by" placeholder for the clinician to complete after appraisal.

**Reference integrity:** DOIs and article metadata are retrieved programmatically from PubMed and verified against source records. The system is instructed never to fabricate or reconstruct identifiers from memory.

**Traceability:** Each review document includes an AI system metadata line recording the skill version, model identifier, and review date. A local evaluation register (`reviews/evaluation_register.csv`, gitignored) logs review outcomes and recommendation counts for ongoing quality monitoring.

## Clinical Content Rules

- Always include evidence grades inline (e.g., "BTS Grade 1C", "NICE Strength: Strong")
- Frame recommendations in UK NHS context (MHRA, NICE TAs, UK registries)
- Numbered in-text citations in square brackets: `[1]`, `[2, 3]`
- DOIs must be copied verbatim from `get_article_metadata` — never reconstructed from memory
- Target 15-30 high-quality references per review
