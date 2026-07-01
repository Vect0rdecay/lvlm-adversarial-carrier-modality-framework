# Structural Tables, Flowcharts, and Records

Ontology class: B (Structural)

Maps from prompt-carriers #3 (Flowcharts & Diagrams), #4 (Tables & Spreadsheets), and #6 (Financial Records).

## Threat relevance

Flowcharts, tables, and financial layouts carry meaning in structure: nodes and edges, rows and columns, field relationships. A model asked to "summarize this table" or "follow the diagram" reconstructs intent through parsing and relational reasoning, not from any single token. Typographic attacks that split instructions across tiles are a special case of the same pattern.

## Literature examples

| Source | Attack / technique | Carrier | Binding pattern |
|---|---|---|---|
| [FigStep / FigStep-Pro](https://arxiv.org/abs/2311.05608) | Typographic visual prompt | Text rendered as image; Pro splits across tiles | "Complete the steps in the image" |
| [CrossInject](https://arxiv.org/abs/2504.14348) | Visual latent + layout | Structured adversarial cues in generated/perturbed images | Textual command grounds visual structure |
| [Prompt Injection 2.0](https://arxiv.org/abs/2507.13169) | Document/page images in agentic RAG | PDF pages, charts, screenshots as retrieved structure | Agent treats retrieved page as trusted context |
| [Cross-Agent Provenance Defense](https://arxiv.org/abs/2512.23557) | Structural unit tagging (defense) | OCR regions, patches, metadata as separate trust units | Sanitizer labels each structural element |

Tables, spreadsheets, and financial records (prompt-carriers #4, #6) fit this class on principle: meaning lives in relationships between cells and fields. Published LVLM attacks targeting pure tabular layout remain thin compared to typographic and document-image work.

## Open gaps

- Direct table-extraction and spreadsheet jailbreak papers are scarce; most structural examples come from typographic images and document RAG.
- Financial-record carriers are backlog until a clear primary citation appears.
