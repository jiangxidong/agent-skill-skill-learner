---
name: skill-learner
description: >
  Analyzes Agent Skills repos (containing SKILL.md files) and generates learning documentation
  including User Guides with prompt templates and Practice guides with real-world examples.
  Use this skill when the user provides local repo paths or mentions wanting to learn, understand,
  or evaluate Agent Skills repos. Triggers on phrases like "learn this skill", "help me understand
  this skill repo", "analyze these agent skills", "generate guide for this skill", or when the user
  provides paths to directories containing SKILL.md files. Also triggers when comparing multiple
  skill repos.
---

# Skill Learner

Analyzes Agent Skills repos and generates structured learning documentation — User Guides with
actionable prompt templates, and Practice guides with real-world implementation examples.

## When to Use

- User provides one or more local paths to Agent Skills repos (directories containing SKILL.md)
- User asks to learn, understand, or evaluate Agent Skills
- User wants to compare multiple skill repos
- User mentions "agent skill", "SKILL.md", "skill repo" in the context of learning

## Workflow

### Phase 1: Explore Repos

For each provided repo path, spawn a parallel Explore subagent to thoroughly analyze:

1. **Repo structure** — README, LICENSE, directory layout
2. **Skill inventory** — All SKILL.md files and their frontmatter (name, description)
3. **Skill content** — Read every SKILL.md body: workflow steps, checklists, examples
4. **References** — Count and summarize files in each `references/` directory
5. **Supporting files** — Scripts in `scripts/`, templates in `assets/`, agent configs in `agents/`
6. **Dependencies** — Any required tools, MCPs, or external services

Report back: skill count, skill names, category breakdown, depth of reference material.

### Phase 2: Interview User

Use the `AskUserQuestion` tool to collect requirements. **Complete the interview in at most 2 calls**
(the tool supports 1-4 questions per call, so batch aggressively).

**Interview dimensions** (skip any already answered by user context or obvious from Phase 1):

1. **Developer background** — Role, experience level with the skill's tech stack
2. **AI coding tool** — Which tool they use (Claude Code, Cursor, Codex, etc.)
3. **Coverage scope** — If repo has multiple skills, which ones to cover (all / subset / let AI recommend). Skip for single-skill repos.
4. **Output location** — Where to save the generated docs
5. **Practice case preference** — What type of example project (let AI decide by default)
6. **Existing projects** — Whether they have a real project to base examples on
7. **Prompt language** — English, Chinese, or mixed for prompt templates
8. **Familiarity with Agent Skills** — Whether to include foundational concepts

**Rules:**
- Batch remaining questions into 1-2 AskUserQuestion calls (max 4 questions each)
- If 4 or fewer questions remain after skipping, use a single call
- Never ask a question whose answer is already known from conversation context or Phase 1 findings

### Phase 3: Generate Documents

Spawn parallel subagents to generate all documents simultaneously.

**Document set rules (self-adaptive):**

| Input | Documents Generated |
|-------|-------------------|
| Single repo | User Guide + Practice |
| Multiple repos | User Guide + Practice for each, plus one Comparison doc |

**For each document, read `references/doc-templates.md`** to follow the standard structure.
**For styling, follow `references/style-guide.md`** strictly.

#### User Guide Structure

For each repo, generate a User Guide containing:

- **Overview** — Repo positioning, author, core value proposition
- **Quick Start** — Installation for the user's specific AI tool, directory structure
- **Skills Overview** — For multi-skill repos: table with name, category, core function, use case. For single-skill repos: replace with a "Skill at a Glance" summary box (name, category, trigger phrases, core value in a compact format)
- **Per-Skill Detail** — For each skill:
  - Function summary (one sentence)
  - Use cases (when to trigger)
  - Core value (what benefit it provides)
  - How to invoke (specific to user's AI tool)
  - 2-3 Prompt templates (copy-paste ready, in code blocks, using a concrete fictional scenario — NEVER use unfilled placeholders like [YOUR_X] or [INSERT_Y]; instead fill in realistic example values)
  - Prerequisites and limitations
- **Recommended Strategy** — Skill combinations by development phase (for single-skill repos: usage scenarios by development phase instead of combinations)

#### Practice Guide Structure

For each repo, generate a Practice guide containing:

- **Case Background** — Fictional but realistic project that naturally exercises all skills in the repo
- **Development Flow Overview** — Table/diagram mapping phases to skills
- **Step-by-Step Phases** — Each phase includes:
  - Goal description
  - Skills used
  - Copy-paste Prompt (in code block)
  - Expected output sample (in code block, showing realistic AI responses)
- **Key Takeaways** — Lessons and methodology summary

The practice case should be designed so every skill in the repo gets used at least once, in a
logical order that mirrors a real development workflow.

**Scaling strategy for large repos (10+ skills):**
- Do NOT try to force all skills into a single linear story
- Instead, design **one overarching project** with **multiple parallel workstreams** (e.g., for a PM skills repo: Strategy workstream, Discovery workstream, Execution workstream, Analytics workstream)
- Each workstream is a self-contained mini-case using the skills in that category
- Include a **cross-workstream integration phase** at the end showing how outputs connect
- Every skill must appear at least once; popular skills may appear in multiple workstreams
- The prompt templates in each workstream phase must reference concrete artifacts from prior steps to feel cohesive
- **Self-check:** After drafting, verify that every skill from Phase 1 appears at least once in the practice content. Missing a skill? Slot it into the most natural workstream phase.

#### Comparison Document (multi-repo only)

When analyzing multiple repos, generate a comparison covering:

- **One-line summary** — Metaphor capturing each repo's positioning
- **Basic info table** — Author, skill count, scope, platform, install method
- **Feature overlap analysis** — What's shared vs unique
- **Depth vs breadth** — Which repo goes deeper, which covers more ground
- **Usage experience** — Invocation style, output format, workflow differences
- **Scenario recommendations** — Which repo for which use case
- **Best combo strategy** — How to use them together
- **Decision matrix** — Quick-reference table: scenario → recommended repo → reason

### Phase 4: Deliver

Confirm all files are written. Present a summary table with file paths and brief descriptions.
Offer to adjust any document.

## Important Notes

- Always explore repos BEFORE interviewing — informed questions are better questions
- Launch all document generation subagents in parallel for speed
- Practice cases should feel realistic, not contrived — choose project types that naturally
  exercise the skills
- Prompt templates should be immediately usable, not abstract
- When a repo has many skills (10+), the practice case needs careful phase design to cover all
  skills without feeling forced
