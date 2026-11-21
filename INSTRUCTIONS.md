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

Uses `claude-sonnet-5-5` with read/write file access only — no shell execution.

## Tracing with Langfuse

Sessions in this project are traced to Langfuse (turn traces, generation spans, tool spans, grouped by session).

1. Create a Langfuse project (https://langfuse.com/cloud) and copy the API keys from Settings > API Keys.
2. Install the plugin, then restart Claude Code. It prompts for `LANGFUSE_PUBLIC_KEY`, `LANGFUSE_SECRET_KEY`, `LANGFUSE_BASE_URL`:
   ```bash
   claude plugin marketplace add langfuse/Claude-Observability-Plugin
   claude plugin install langfuse-observability@langfuse-observability
   pip install "langfuse>=4.0,<5"
   ```
3. `.claude/settings.json` already sets `TRACE_TO_LANGFUSE=true` (no secrets). If you use the manual hook instead of the plugin, put keys in `.claude/settings.local.json`, which is gitignored.
4. The base URL (EU `https://cloud.langfuse.com`, US `https://us.cloud.langfuse.com`, or self-hosted) comes from the plugin config; do not also set it in `.claude/settings.json`, or key/host mismatches cause 401s.

Note: there is no redaction; text is only truncated to `CC_LANGFUSE_MAX_CHARS`. Don't trace sensitive repos.
