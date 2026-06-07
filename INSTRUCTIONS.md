# doc-writer Agent — Instructions

A Claude Code subagent that generates documentation for your codebase. Invoked automatically when you ask Claude to document code, add JSDoc, write README sections, or create API docs.

## Setup

1. Copy `CLAUDE.md` into your project's root (or `.claude/` folder).
2. Open the project in Claude Code.
3. Ask Claude to document any file, function, or module — the `doc-writer` agent will be selected automatically.

## Usage Examples

```
"Document the auth module"
"Add JSDoc to all functions in utils.js"
"Write a README section for the API endpoints"
"Document the request/response shape for POST /users"
```

## What the Agent Does

- Reads the target file(s) and checks the project's existing documentation style
- Adds function/method docs (description, params, return value, example)
- Adds inline comments explaining non-obvious logic (WHY, not WHAT)
- Documents API endpoints with method, path, request/response shapes, and auth requirements
- Flags unclear code for refactoring without changing functionality

## Model

Uses `claude-sonnet-4-5-20250929` with read/write file access only — no shell execution.
