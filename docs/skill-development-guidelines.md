# Skill Development Guidelines

This repository treats each skill as a small, focused operating guide for an AI assistant working with real network infrastructure.

## Required structure

Each skill should contain:

1. A clear purpose.
2. Explicit trigger conditions.
3. Required inputs and assumptions.
4. A repeatable workflow.
5. Platform-specific boundaries.
6. Validation and failure handling.
7. A safety section.
8. A predictable output format.
9. Short examples of valid requests.

## Writing style

- Use imperative language: `Collect`, `Validate`, `Generate`, `Review`, `Test`.
- State uncertainty explicitly.
- Ask for missing facts instead of guessing.
- Separate observed facts, assumptions, recommendations, and executed changes.
- Prefer precise terms such as `desired state`, `dry run`, `idempotent`, `rollback`, and `blast radius`.
- Avoid unexplained vendor jargon.

## Testing a skill

Test every skill with at least:

- A normal request with complete inputs.
- A request with missing platform or inventory details.
- A read-only request.
- A request that would be destructive without confirmation.
- A request containing an unsafe secret or credential.

The expected behavior is to ask focused questions, avoid invented values, and produce a safe plan before producing executable changes.
