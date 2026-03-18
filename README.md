# Clinical Protocol Reviewer

A Claude skill that reviews clinical protocols against current national guidelines and published evidence, producing a structured Word document with evidence-graded recommendations.

## What It Does

Upload a clinical protocol (PDF or Word) and the skill will:

1. **Read and parse** the protocol, extracting key clinical topics, drugs, doses, and thresholds
2. **Search national guidelines** (BTS, NICE, KDIGO, BSH, SIGN, etc.) via web search
3. **Search published evidence** using both PubMed (keyword/MeSH) and Scholar Gateway (semantic search)
4. **Cross-reference** the protocol against current guidelines and recent evidence
5. **Generate a review document** (.docx) with section-by-section analysis and actionable recommendations
6. **Export references** as BibTeX (.bib) and a PMID list (.txt) for Zotero import

## Output Files

Each review produces four files:

| File | Purpose |
|------|---------|
| `*_Review_*.md` | Markdown source for the review |
| `*_Review_*.docx` | Formatted Word document (via pandoc) |
| `*_References.bib` | BibTeX file for Zotero/reference manager import |
| `*_PMIDs.txt` | PMID list for Zotero bulk import |

## Requirements

- [Claude Desktop](https://claude.ai/download) or another Claude client with MCP connector support
- The following MCP connectors enabled:
  - **PubMed** — literature search and article metadata
  - **Scholar Gateway** — semantic search across peer-reviewed literature
- **pandoc** — for markdown to .docx conversion
- The skill is designed for **UK NHS context** (references MHRA, NICE TAs, UK registries)

## Installation

This skill requires the **Claude Desktop app** (macOS or Windows).

### Option 1: Install from .skill file (recommended)

1. Download `protocol-reviewer.skill` from the [Releases](../../releases) page
2. Double-click the file — Claude Desktop will prompt you to install it
3. The skill and all its bundled resources (templates, reference docs) will be installed automatically

### Option 2: Install from repository

1. Download or clone this repository:
   ```bash
   git clone https://github.com/<your-username>/protocol-reviewer.git
   ```
   Or download as a ZIP from the green **Code** button above and extract it.
2. In Claude Desktop, open **Settings → Skills** and add the path to the extracted `protocol-reviewer` folder

### After installation

1. **Set up your reviewer profile** — copy the example config and fill in your details:
   ```bash
   cp reviewer_config.example.yaml reviewer_config.yaml
   ```
   Edit `reviewer_config.yaml` with your name, professional title, and optionally your institution. This is used in the transparency disclaimer of every review document. The file is gitignored so your personal details stay local.

2. **Enable the required MCP connectors** in Claude Desktop (Settings → Connectors):
   - **PubMed** — for literature search, article metadata, and full text retrieval
   - **Scholar Gateway** — for semantic search across peer-reviewed literature

3. **Install pandoc** for markdown-to-docx conversion: `brew install pandoc` on macOS or `sudo apt install pandoc` on Linux.

The skill also uses **WebSearch** (enabled by default) for national guideline lookups.

## Example Prompts

- *"Review this protocol against the latest BTS guidelines and PubMed evidence"*
- *"Is this protocol still current? Check the latest literature and give me recommendations"*
- *"Update this guideline — check NICE and recent RCTs, and export references for Zotero"*

## AI Use & Governance (ISO 42001)

This tool uses AI-assisted evidence synthesis to support clinical protocol review. AI outputs are advisory only and must be critically appraised by a consultant-level clinician before informing protocol changes. The AI system is Claude (Anthropic), accessed via Claude Desktop with PubMed and Scholar Gateway integrations.

Every review document includes a transparency disclaimer (section 6) with an AI system metadata line recording the skill version, model identifier, and review date. References are retrieved programmatically from PubMed and verified against source metadata — the system never fabricates identifiers.

A local evaluation register (`reviews/evaluation_register.csv`, gitignored) logs each review's outcomes and recommendation counts for ongoing quality monitoring and audit. This file stays on your machine and is never pushed to the repository.

See [`CLAUDE.md`](CLAUDE.md) for the full AI use policy, including intended use, human oversight requirements, and reference integrity controls.

## Trigger Phrases

The skill activates on phrases like: "review this protocol", "update this guideline", "check this against latest evidence", "is this protocol still current", "compare to BTS/NICE/SIGN guidelines", or any request involving a clinical document that needs reviewing.
