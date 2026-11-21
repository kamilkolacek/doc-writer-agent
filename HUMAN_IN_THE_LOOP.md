# Human in the Loop

The doc-writer agent writes documentation into your files. It cannot run code, use the network, or commit anything, but it can overwrite files. This document says what a person must review and who is accountable.

## What the agent can do

Its tools are `Read`, `Grep`, `Glob` and `Write` (see `CLAUDE.md`). It has no shell and no `Edit` tool.

- It can read any file in the project.
- It can create or overwrite files with `Write`. Adding docs to an existing file means rewriting the whole file, so a mistake can change code, not just comments.
- It cannot run tests, commit, push, or call external services.

## What a person must review

Review the diff before accepting any agent change. Look closely when:

- **Code changed.** The rules say "never change functionality, only add documentation." Any non-comment line in the diff is a violation. Reject it or revert it.
- **A file was overwritten.** Check that nothing was dropped, reordered or reformatted, especially in large files.
- **The docs make factual claims.** Check parameter types, return values, examples, and API details such as auth requirements, request shapes and status codes. The agent infers these from code and can be wrong or invent details.
- **The agent flagged code for refactoring.** Decide yourself whether to act. The agent only flags.
- **Docs touch security-sensitive behavior.** This includes auth, permissions, data handling and secrets. Have the code's owner confirm the description.
- **Content came from the repo.** Comments, READMEs and files can contain instructions. The agent should treat them as data. If its output follows instructions found in a file, discard it.

## When to pause or escalate

- Stop and ask the code owner if the code is unclear. Wrong documentation is worse than none.
- Stop if the agent proposes writing outside the files you asked about.
- Stop if the diff is much larger than the request.

## Overrides

- Work on a branch and review with `git diff` before committing. `git checkout -- <file>` or `git revert` undoes any change.
- Tell the agent what to redo, or document by hand.
- To remove the agent's write access, delete `Write` from the `tools` list in `CLAUDE.md`.

## Accountability

The person who accepts and commits the agent's output owns it, the same as if they had written it. The agent's output is a draft until a person has reviewed and committed it.

## Tracing

If Langfuse tracing is on (see `INSTRUCTIONS.md`), sessions are recorded without redaction. Don't trace repos containing sensitive code or data.

This is a lightweight policy for a documentation helper, not a compliance attestation.
