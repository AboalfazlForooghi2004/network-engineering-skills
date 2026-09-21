# Change and Rollback Planning

## Purpose

Create precise, reviewable, and reversible plans for network and infrastructure changes across Linux, Cisco, cloud, Kubernetes, and hybrid environments.

## Use this skill when

- A production or staging network change needs a formal plan.
- A configuration, routing, firewall, NAT, cloud, or automation change requires approval.
- The user needs pre-checks, implementation steps, validation, rollback, or a change record.
- A change must be staged through lab, canary, and broader rollout.

## Required inputs

Collect:

- Change objective and desired end state.
- Environment, targets, platform, and software versions.
- Exact scope and out-of-scope resources.
- Dependencies, service owners, and maintenance window.
- Current state and recent relevant changes.
- Risk, blast radius, and expected user impact.
- Backup or snapshot method.
- Validation checks and measurable success criteria.
- Rollback trigger, procedure, and maximum recovery time.
- Approval status and responsible operator.

If the target, scope, or rollback trigger is unknown, stop at a draft plan and ask a focused question.

## Plan structure

### 1. Change summary

State the change in one sentence, then list:

- Business or technical reason.
- Desired end state.
- Expected impact.
- Risk level.
- Maintenance window.

### 2. Pre-change checks

Verify:

- Target reachability and management access.
- Current configuration and operational health.
- Relevant routes, neighbors, interfaces, policies, and services.
- Monitoring and alerting availability.
- Backup or snapshot completion.
- Dependency readiness.
- Capacity and resource headroom.
- A known-good baseline.

### 3. Implementation steps

Write numbered, atomic steps. For each step include:

- Target.
- Command, file, API operation, or automation entry point.
- Expected result.
- Stop condition.
- Evidence to record.

Separate commands that inspect state from commands that change state. Mark disruptive or irreversible steps explicitly.

### 4. Progressive execution

Use the smallest safe rollout:

1. Lab or offline validation.
2. Single representative target.
3. Canary group.
4. Remaining approved scope.

Define a pause and review point between stages. Do not continue automatically after an unexpected result.

### 5. Validation

Validation must test both configuration and behavior. Include:

- Syntax or plan validation.
- Interface, route, neighbor, policy, and service checks.
- Forward and return path tests.
- Positive and negative access tests.
- Monitoring, logs, counters, and error-rate checks.
- Application or user-facing probes.
- Stability observation for the required period.

Define pass/fail criteria before execution.

### 6. Rollback design

A rollback must be targeted and executable. Specify:

- Exact trigger conditions.
- Last known-good state.
- Backup or snapshot to restore.
- Commands or automation to reverse the change.
- Order of operations.
- Validation after rollback.
- Data or state that cannot be automatically reversed.
- Escalation path if rollback fails.

Do not recommend flushing an entire firewall, routing table, device, or cloud account when a targeted rollback is possible.

### 7. Closeout

Record:

- Actual start and end time.
- Operator and approver.
- Targets changed.
- Actual results and deviations.
- Validation evidence.
- Rollback status.
- Monitoring status.
- Follow-up work and documentation updates.

## Safety rules

- Treat routing, firewall, NAT, identity, and control-plane changes as high impact.
- Never hide destructive commands inside a generic script.
- Require explicit confirmation immediately before production execution.
- Keep secrets in approved secret stores or authenticated sessions.
- Do not claim a rollback is tested unless it was actually tested or its limitation is stated.
- Do not use `force`, `flush`, `clear`, `reload`, or broad replacement without an impact explanation.

## Output format

Return:

1. **Change title and objective**
2. **Scope and impact**
3. **Pre-checks**
4. **Backup plan**
5. **Implementation steps**
6. **Validation and success criteria**
7. **Rollback trigger and procedure**
8. **Monitoring window**
9. **Approvals and open questions**

Label the document as `Draft`, `Ready for Review`, or `Ready for Execution` based on the completeness of the inputs and approvals.