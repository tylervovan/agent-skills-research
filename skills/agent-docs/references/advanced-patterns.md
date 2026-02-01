# Advanced Documentation Patterns for AI Agents

Detailed patterns for specific documentation scenarios.

## Table of Contents

1. [RAG-Optimized Structure](#rag-optimized-structure)
2. [Multi-Framework Docs](#multi-framework-docs)
3. [API Documentation](#api-documentation)
4. [Troubleshooting Sections](#troubleshooting-sections)
5. [Version-Specific Content](#version-specific-content)

---

## RAG-Optimized Structure

### Chunk Size Sweet Spot

Most RAG systems use 500-1500 token chunks. Structure accordingly:

```markdown
## Feature X                    ← ~100-200 tokens ideal header context

Brief description (1-2 sentences).

### Usage
Code example here.

### Options
| Option | Type | Default | Description |
|--------|------|---------|-------------|
...
```

### Semantic Boundaries

Place related content under the same header hierarchy:

**Good:**
```markdown
## User Authentication
### Password Login
### OAuth2 Flow
### Session Management
```

**Bad:**
```markdown
## Password Login
## OAuth2 Flow
## Sessions
```

The good version keeps auth-related content retrievable as a unit.

### Keyword Density

Include relevant keywords naturally — RAG searches by semantic similarity:

```markdown
## Database Connection Pooling

Configure connection pools for PostgreSQL and MySQL databases.
Pool size, timeout, and retry settings.
```

Keywords: database, connection, pool, PostgreSQL, MySQL, timeout, retry

---

## Multi-Framework Docs

When supporting multiple frameworks, use parallel structure:

```markdown
## Installation

### React
npm install @lib/react

### Vue
npm install @lib/vue

### Svelte
npm install @lib/svelte
```

Or use a reference file per framework:
```
references/
├── react.md
├── vue.md
└── svelte.md
```

With SKILL.md pointing to the appropriate file:

```markdown
## Framework-Specific Setup

- **React**: See [references/react.md](references/react.md)
- **Vue**: See [references/vue.md](references/vue.md)
```

---

## API Documentation

### Endpoint Template

```markdown
## GET /api/users

Retrieve users with optional filters.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| limit | int | No | Max results (default: 20) |
| offset | int | No | Pagination offset |
| status | string | No | Filter: active, inactive |

**Response:**
```json
{
  "users": [...],
  "total": 100,
  "hasMore": true
}
```

**Errors:**
- `401` — Invalid or missing auth token
- `403` — Insufficient permissions
```

### Include Real Examples

Don't just document — show:

```markdown
## Create User

```bash
curl -X POST https://api.example.com/users \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com"}'
```

Response:
```json
{"id": "usr_123", "name": "Alice", "created": "2024-01-15T10:00:00Z"}
```
```

---

## Troubleshooting Sections

Structure for fast pattern matching:

```markdown
## Troubleshooting

### "Connection refused" error

**Cause:** Database not running or wrong port.

**Fix:**
1. Check if PostgreSQL is running: `pg_isready`
2. Verify `DATABASE_URL` port matches PostgreSQL config
3. Check firewall rules if remote database

### "Invalid token" error

**Cause:** Expired or malformed JWT.

**Fix:**
1. Check token expiration: `jwt decode $TOKEN`
2. Regenerate token if expired
3. Verify `JWT_SECRET` matches between services
```

Agents can pattern-match error messages to solutions.

---

## Version-Specific Content

### Conditional Sections

```markdown
## Migration Guide

### v2.x → v3.x

Breaking changes:
- `config.timeout` renamed to `config.requestTimeout`
- Removed deprecated `legacyMode` option

### v1.x → v2.x

Breaking changes:
- Changed from CommonJS to ESM
```

### Feature Flags

```markdown
## Experimental Features

> **Note:** Requires `--experimental` flag or `ENABLE_EXPERIMENTAL=1`

### Stream Processing (experimental)
...
```

Clear labeling helps agents understand availability constraints.
