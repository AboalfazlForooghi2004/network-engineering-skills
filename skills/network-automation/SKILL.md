# Network Automation

## Purpose

Provide safe, production-aware guidance for designing, reviewing, generating, and validating network automation across Linux, Cisco, cloud, and mixed environments.

## Use this skill when

- Designing automation for network devices, servers, or cloud networking.
- Choosing between Ansible, Nornir, Netmiko, Scrapli, Terraform, Jinja2, or a combined workflow.
- Reviewing an existing automation repository or playbook.
- Converting a manual network procedure into a repeatable workflow.
- Adding dry-run, idempotency, backup, validation, rollback, Git, or CI/CD controls.
- Generating device configuration only after the target platform and desired state are known.

## Operating principles

1. Start with discovery and planning, not command generation.
2. Treat inventory, platform, access method, and desired state as required inputs.
3. Operate in read-only or dry-run mode by default.
4. Never guess vendor syntax, interface names, IP addresses, credentials, or device capabilities.
5. Separate planning, rendering, execution, validation, and rollback.
6. Make every workflow idempotent where the underlying platform supports it.
7. Keep secrets outside source code, templates, logs, pull requests, and generated artifacts.
8. Require explicit confirmation before production changes or destructive actions.
9. Produce evidence for every change: inputs, rendered output, command result, validation result, and rollback status.
10. Prefer small, reviewable commits and pull requests over large untraceable changes.

## Required inputs

Collect the following before generating executable automation:

- Objective and expected end state.
- Target environment: lab, staging, or production.
- Inventory: hosts, groups, regions, sites, tenants, and device roles.
- Platform and version: Linux distribution, Cisco OS family, cloud provider, or other vendor.
- Connection method and privilege model. Do not request or expose credentials.
- Existing configuration or relevant state.
- Scope: devices, interfaces, VLANs, routes, policies, resources, or files affected.
- Constraints: maintenance window, availability, dependencies, compliance, and performance.
- Backup method and storage location.
- Validation checks and measurable success criteria.
- Rollback method and the condition that triggers rollback.
- Approval status for the proposed change.

If a required input is missing, ask a focused question. Do not substitute an invented default for a production value.

## Standard workflow

### 1. Define the change

Translate the request into a concise change statement:

- Current state.
- Desired state.
- In-scope targets.
- Out-of-scope targets.
- Expected impact.
- Risk and blast radius.

### 2. Normalize the inventory

Validate that the inventory identifies the correct targets and separates environments. Check for duplicate devices, missing platform data, unreachable targets, and inconsistent group membership.

### 3. Select the toolchain

Choose the smallest toolchain that fits the problem:

- Use Ansible for declarative, repeatable tasks across an inventory.
- Use Nornir for Python-driven orchestration, concurrency, and custom control flow.
- Use Netmiko for straightforward SSH-based device interaction and legacy compatibility.
- Use Scrapli for typed, fast, structured CLI transport to supported network platforms.
- Use Terraform for declarative cloud and infrastructure resource lifecycle management.
- Use Jinja2 for controlled rendering from validated data; do not use templates as a substitute for inventory validation.
- Use Git and CI/CD for review, linting, testing, policy checks, artifact storage, and controlled promotion.

Explain the selection and identify any trade-offs.

### 4. Design the data model

Define structured inputs before writing tasks or templates. Keep inventory data, platform variables, secrets references, policy data, and rendered configuration separate.

Use explicit names for:

- Device or resource identity.
- Platform and version.
- Interfaces and logical roles.
- Addresses, prefixes, VLANs, VRFs, and routing policies.
- Desired state.
- Validation checks.
- Rollback metadata.

### 5. Build a dry-run path

The first execution path should be non-destructive:

- Render or preview the intended change.
- Show a diff against the current state when possible.
- Identify unsupported or ambiguous fields.
- Fail before connecting when validation fails.
- Avoid sending configuration commands during preview.

### 6. Implement idempotency

A second run with no input changes must produce no unintended change. Check for:

- Stable resource names.
- Deterministic rendering.
- State-aware modules or APIs.
- Safe `replace` or reconciliation behavior.
- No duplicate routes, VLANs, ACL entries, users, or cloud resources.
- Explicit handling of absent, present, and removed state.

### 7. Add backup and rollback

Before execution:

- Capture the relevant running and intended state.
- Store backups securely and label them with target, timestamp, and change identifier.
- Verify that the backup is complete and retrievable.
- Define the smallest safe rollback unit.

Rollback must be specific to the change. Do not recommend flushing an entire device, routing table, firewall, or cloud account when a targeted rollback is possible.

### 8. Execute progressively

Use staged execution:

1. One lab target.
2. A representative staging target.
3. A small production canary group.
4. The remaining approved scope.

Stop on validation failure, unexpected output, connectivity loss, or a blast-radius condition. Do not silently continue.

### 9. Validate the result

Validate both configuration and behavior. Examples include:

- Configuration diff and syntax validation.
- Interface, VLAN, VRF, route, and neighbor state.
- Reachability and return-path checks.
- BGP or OSPF adjacency.
- ACL, NAT, firewall, and policy counters.
- Cloud route, security policy, endpoint, and health status.
- Service-level probes and monitoring signals.

Record actual results instead of claiming success because a command returned zero.

### 10. Report and document

Return:

- Summary of the requested change.
- Assumptions and unresolved questions.
- Selected toolchain and rationale.
- Files or playbooks changed.
- Dry-run or diff result.
- Execution result.
- Validation evidence.
- Backup location or reference.
- Rollback procedure.
- Follow-up items.

## Tool-specific guidance

### Ansible

- Use inventories, groups, variables, roles, and tags deliberately.
- Prefer vendor modules or structured APIs over raw CLI commands.
- Use `--check` and `--diff` where supported.
- Protect secrets with Vault or an external secret manager.
- Make handlers, templates, and tasks safe to run repeatedly.

### Nornir

- Keep inventory and task logic separate.
- Control concurrency to avoid overloading devices or APIs.
- Capture per-host results and failures.
- Use explicit exception handling and stop conditions.
- Add a dry-run mode before enabling configuration tasks.

### Netmiko

- Confirm device type, prompt behavior, privilege level, and timeout.
- Prefer `send_command` for inspection and `send_config_set` only after approval.
- Capture command output and verify the resulting state.
- Do not treat a successful SSH connection as proof that a change succeeded.

### Scrapli

- Use the correct platform driver and transport settings.
- Prefer structured responses and explicit privilege transitions.
- Set timeouts and failure behavior deliberately.
- Test command compatibility against the target OS version.

### Terraform

- Inspect the plan before apply.
- Protect state and use remote state controls where appropriate.
- Avoid broad replacement when a targeted update is possible.
- Use provider and resource versions intentionally.
- Define import, drift detection, and recovery procedures.

### Jinja2

- Keep templates simple and deterministic.
- Validate input data before rendering.
- Avoid hidden logic that changes configuration unexpectedly.
- Test whitespace, ordering, defaults, and empty collections.
- Never put secrets directly in templates.

### Git and CI/CD

- Keep changes small and reviewable.
- Run formatting, linting, unit tests, template validation, and policy checks in CI.
- Store generated artifacts only when they contain no secrets.
- Protect production branches and require review for production changes.
- Make deployments traceable to a commit, change request, and approval.

## Output format

Unless the user requests another format, structure the response as:

1. **Assessment** — restate the goal and identify missing inputs.
2. **Scope and risk** — targets, blast radius, dependencies, and constraints.
3. **Proposed design** — toolchain, data model, workflow, and rationale.
4. **Dry-run or preview** — expected diff, checks, and failure conditions.
5. **Implementation** — files, tasks, templates, or commands, clearly separated from execution.
6. **Validation** — exact checks and success criteria.
7. **Rollback** — targeted recovery steps and trigger conditions.
8. **Open questions** — facts still required before execution.

## Safety boundary

Do not execute or present destructive production automation as routine. Before any production-impacting action, obtain explicit confirmation after showing the scope, expected impact, validation plan, and rollback plan.

Do not include or request passwords, private keys, tokens, enable secrets, or cloud credentials. Refer to secret managers, environment variables, or authenticated sessions instead.

Do not claim that a workflow is idempotent, tested, portable, or safe unless the evidence supports that claim.
