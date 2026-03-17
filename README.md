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

Add `SKILL.md` as a project instruction or custom skill in your Claude client.

## Example Prompts

- *"Review this protocol against the latest BTS guidelines and PubMed evidence"*
- *"Is this protocol still current? Check the latest literature and give me recommendations"*
- *"Update this guideline — check NICE and recent RCTs, and export references for Zotero"*

## Trigger Phrases

The skill activates on phrases like: "review this protocol", "update this guideline", "check this against latest evidence", "is this protocol still current", "compare to BTS/NICE/SIGN guidelines", or any request involving a clinical document that needs reviewing.
