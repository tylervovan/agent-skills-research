---
name: agent-docs
description: Create documentation optimized for AI agent consumption. Use when writing SKILL.md files, README files, API docs, or any documentation that will be read by LLMs in context windows. Helps structure content for RAG retrieval, token efficiency, and chunking.
---

# Agent Docs

Write documentation that AI agents can efficiently consume. Optimized for context windows, RAG retrieval, and chunking.

## Core Principles

### 1. Structure for Chunking

AI retrieval systems split docs at headers. Make each section self-contained:

```markdown
## Database Setup          ← Chunk boundary

Prerequisites: PostgreSQL 14+, Node.js 18+

1. Create database...
2. Run migrations...

## API Configuration       ← Chunk boundary
```

**Rules:**
- Each H2/H3 section should make sense in isolation
- Front-load key info (chunkers often truncate)
- Use clear, descriptive headers (agents search by header text)

### 2. Inline Over Links

Agents can't automatically follow external links. Each link = a tool call + more tokens.

| Approach | Tokens on Load | Agent Usefulness |
|----------|---------------|------------------|
| Full inline | ~12K | ✅ Complete context |
| Sparse + links | ~2K | ❌ Must fetch each link |
| Chunked sections | ~1-3K per chunk | ✅ RAG retrieves relevant parts |

**Do:** Include the actual information inline
**Don't:** Say "see OWASP docs for details" without including the relevant details

### 3. Token-Efficient Patterns

**Good:**
```markdown
## Auth Setup
Use JWT with RS256. Set `JWT_SECRET` env var.
```

**Bad:**
```markdown
## Authentication Setup Guide
This comprehensive section will walk you through the process of setting up authentication for your application. Authentication is an important security measure that...
```

Skip preambles. Agents are smart — they don't need hand-holding.

### 4. Explicit Over Implicit

State assumptions and requirements directly:

```markdown
## Deploy to AWS

**Requires:** AWS CLI configured, ECR repo created, ECS cluster running

1. Build image: `docker build -t app .`
2. Push: `docker push $ECR_URI`
```

### 5. Examples Beat Explanations

```markdown
## Query Syntax

Filter by date:
`GET /api/events?after=2024-01-01&before=2024-12-31`

Filter by status:
`GET /api/events?status=active,pending`
```

One example is worth 100 words of explanation.

## Document Structure Template

```markdown
# [Tool/Topic Name]

One-line description of what this does.

## Quick Start

Minimal working example. Get something running in <30 seconds.

## Core Concepts

Only if truly necessary. Most agents already know general concepts.

## [Task Category 1]

### Subtask A
Concrete steps, code examples.

### Subtask B
More steps, more examples.

## [Task Category 2]
...

## Troubleshooting

Common errors and fixes. Agents love these.
```

## Anti-Patterns to Avoid

1. **"See external docs"** — Agents can't browse autonomously
2. **Walls of prose** — Hard to chunk, agents skip to examples anyway
3. **Nested link chains** — "See X, which references Y, which links to Z"
4. **Redundant sections** — "What is [X]?" when the agent already knows
5. **TOC-only docs** — A table of contents isn't documentation

## When to Link Externally

Only when:
- Content changes frequently (API versions, release notes)
- Legal/compliance requires canonical source
- File is genuinely too large (>50KB) and must be fetched on-demand

Even then, include a summary of key points inline.

## Advanced Patterns

For detailed guidance on specific scenarios, see [references/advanced-patterns.md](references/advanced-patterns.md):

- RAG-optimized chunk sizing
- Multi-framework documentation
- API documentation templates
- Troubleshooting section structure
- Version-specific content handling

## Validation Checklist

Before finalizing docs:

- [ ] Each H2 section is self-contained
- [ ] No external links without inline summaries
- [ ] Examples included for every major feature
- [ ] Prerequisites stated explicitly
- [ ] No marketing fluff or unnecessary preambles
