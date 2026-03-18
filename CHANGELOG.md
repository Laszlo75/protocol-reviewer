# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.0] - 2026-03-18

### Added

- Full text retrieval via `get_full_text_article` for key open-access papers (5-10 per review)
- Guidance on when to retrieve full text vs rely on abstracts and Scholar Gateway passages
- Graceful fallback when full text is unavailable (non-open-access papers)

## [1.2.1] - 2026-03-18

### Added

- Model Requirements section in SKILL.md: specifies Claude Opus 4.6 as the required model
- Extended thinking guidance for evidence synthesis, cross-referencing, and recommendation writing steps

## [1.2.0] - 2026-03-18

### Added

- AI system metadata line in transparency disclaimer (skill version, model ID, review date) for ISO 42001 traceability
- Local evaluation register (`reviews/evaluation_register.csv`) — appended after each review, gitignored
- Step 8 in SKILL.md: append to evaluation register after reference verification
- `.gitignore` to exclude `reviews/` directory from version control
- Traceability section in CLAUDE.md AI Use Policy

### Changed

- Transparency disclaimer now includes dynamic metadata line populated at review time
- Workflow expanded from 7 steps to 8 (added evaluation register step)

## [1.1.0] - 2026-03-18

### Added

- Transparency disclaimer (section 6) in every review document, naming the appraising clinician and disclosing AI involvement (ISO 42001)
- AI Use Policy section in CLAUDE.md covering system identity, intended use, human oversight, transparency, and reference integrity
- AI Use & Governance summary in README.md with link to full policy in CLAUDE.md

### Changed

- Review document section numbering: References moved from section 6 to section 7 to accommodate the disclaimer
- SKILL.md updated with mandatory transparency disclaimer instructions and exact wording

## [1.0.0] - 2026-03-17

### Added

- 7-step clinical protocol review workflow (SKILL.md)
- PubMed MCP and Scholar Gateway dual search strategy
- Pandoc-based markdown-to-docx conversion with styled reference template
- BibTeX (.bib) and PMID list (.txt) export for Zotero import
- PubMed search strategy guide (references/pubmed_strategy.md)
- Review document template with YAML frontmatter (references/document_template.md)
- Reference integrity verification step (DOI spot-check, title-PMID consistency)
- Evaluation scenarios for ABOi transplant, perioperative anticoagulation, and CMV prophylaxis
