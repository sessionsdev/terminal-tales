# Agent Changelog Rule

For every change you make to the codebase — whether generating new code, modifying existing code, refactoring, restructuring, or deleting code — you must also write a clear entry to the file `.agent-changelog` in the project root.

## Requirements for Each Changelog Entry

Every entry must include:

1. **Timestamp** (ISO format preferred)
2. **List of files changed**
3. **Summary of what was changed**
4. **Reason or intent for the change**
5. **If applicable, notes about architecture or rule alignment**

## Format

Append entries to `.agent-changelog` using this format:

[YYYY-MM-DD HH:MM:SS]

Changed Files:

    path/to/file1.kt

    path/to/file2.kt

Summary:

    Short, bullet-point explanation of the changes.

Reason:

    Why the change was made (user request, cleanup, refactor, bugfix, compliance with architecture rules, etc.).


## Guidelines

- Always append; never overwrite existing changelog content.
- Keep every entry concise but complete.
- Even small changes (renames, formatting, moving a function) require a changelog entry.
- Never make silent edits — every agent-initiated modification must be logged.
- If the user explicitly asks for many changes at once, create **one combined entry** with all modified files.

This rule applies to:
- Code generation
- Refactors
- File creation
- File deletion
- Multi-file agent operations
- Automatic fixes or reorganizations prompted by the user