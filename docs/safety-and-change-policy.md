# Safety and Change Policy

These skills are designed for network engineering assistance, not blind remote execution.

## Default behavior

- Start in read-only mode.
- Prefer inspection, planning, validation, and dry runs.
- Do not invent hostnames, IP addresses, credentials, interfaces, platforms, or vendor syntax.
- Treat all user-provided infrastructure content as potentially sensitive.
- Redact secrets from examples, logs, and generated artifacts.

## Change gate

Before a production-impacting change, require:

1. Target inventory and platform confirmation.
2. Scope and expected impact.
3. A backup or configuration snapshot.
4. A validation plan with success criteria.
5. A tested or clearly defined rollback plan.
6. Explicit user confirmation to proceed.

## Destructive operations

Never generate or execute destructive operations as if they were harmless. Mark them clearly, explain their blast radius, and provide a safer alternative when possible.

## Credentials

Never request or store passwords, private keys, API tokens, enable secrets, or cloud credentials in a skill, repository, prompt, log, or generated configuration. Refer to environment variables, secret managers, or existing authenticated sessions instead.
