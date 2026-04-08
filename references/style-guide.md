# Document Style Guide

All generated documents must follow these conventions strictly.

## Language

- **English only**: All content — topic names, key points, dimension names, technical terms,
  conceptual summaries, detailed descriptions, and explanations — must be written in English.
- Prompt templates: English only — both key instructions and context descriptions.

## Formatting

- **Bullet style**: Always use `-`, never `*`
- **Bold lead-ins**: Every list item opens with a short bold phrase for skimmability,
  followed by 1-2 short sentences
- **Short paragraphs**: No wall of text. Keep paragraphs to 2-3 sentences max
- **Clear hierarchy**: Logical heading structure, consistent nesting
- **Information density**: Moderate — enough detail to be useful, not so much it overwhelms
- **No filler**: Go straight to content. No "let me explain" or "as we can see" preambles
- **Tables**: Use tables for comparisons and overviews. Tables can contain short bullet lists

## Code Blocks

- **Prompt templates**: Wrap in plain code blocks (no language tag)
- **Code examples**: Use appropriate language tag (`swift`, `typescript`, `python`, etc.)
- **Expected output samples**: Use code blocks, clearly labeled as simulated output
- **Shell commands**: Use `bash` code blocks

## Data Visualization

- Use Markdown tables for structured comparisons
- Use simple ASCII flow diagrams for workflows when helpful
- Mermaid diagrams are acceptable if the rendering environment supports them

## Document Headers

Every document starts with a level-1 heading (`#`) that clearly identifies the content.
No YAML frontmatter in output documents.
