# Network Documentation and Runbooks

## Purpose

Create accurate, maintainable network documentation and operational runbooks from verified infrastructure information.

## Use this skill when

- Writing HLD, LLD, as-built documentation, topology descriptions, or service handoff documents.
- Converting troubleshooting knowledge into a repeatable runbook.
- Documenting Linux, Cisco, cloud, Kubernetes, security, or hybrid-network architecture.
- Reviewing an existing document for missing assumptions, unsafe steps, or outdated facts.

## Required inputs

Collect:

- Audience and purpose.
- Environment and scope.
- Verified topology, platforms, versions, and ownership.
- Addressing, VLAN, VRF, routing, firewall, DNS, and dependency data.
- Monitoring and alert sources.
- Supported procedures and known failure modes.
- Maintenance, escalation, and approval rules.
- Links to authoritative diagrams, repositories, tickets, or configuration sources.

Do not invent IP addresses, topology links, ownership, SLAs, commands, or recovery times. Mark unknown values as `TBD` or `[Needs confirmation]`.

## Documentation types

### High-Level Design

Describe:

- Goals and non-goals.
- Logical architecture.
- Trust and failure boundaries.
- Major components and dependencies.
- Traffic classes and security model.
- Availability, scale, and recovery objectives.
- Design decisions and trade-offs.

### Low-Level Design

Describe:

- Physical and logical interfaces.
- Address and VLAN plan.
- VRFs and routing tables.
- Protocol parameters and policy.
- Firewall, NAT, ACL, and security rules.
- Service dependencies.
- Monitoring and validation points.
- Implementation and rollback references.

### As-built document

Record what is actually deployed, not what was originally intended. Include the source and date of verification for important facts.

### Runbook

Provide a repeatable procedure for an operator during a known task or incident. Include pre-checks, commands, expected output, decision points, escalation, validation, and rollback.

## Documentation workflow

### 1. Define the document contract

State the audience, purpose, scope, version, owner, last verification date, and authoritative sources.

### 2. Separate facts from interpretation

Use labels such as:

- **Verified fact**
- **Assumption**
- **Design decision**
- **Operational procedure**
- **Known limitation**
- **Needs confirmation**

Never convert an assumption into a fact through polished wording.

### 3. Build the structure

Use a consistent order:

1. Summary.
2. Scope and non-goals.
3. Architecture or procedure.
4. Dependencies.
5. Addressing and naming.
6. Routing and traffic flows.
7. Security controls.
8. Monitoring and alerting.
9. Failure modes and recovery.
10. Validation checklist.
11. Change and rollback references.
12. Ownership and escalation.
13. Revision history.

### 4. Document flows

For each important flow, show:

```text
source → ingress → classification/policy → route or service → egress → destination
return destination → state restoration/policy → return route → source
```

Mention interfaces, VRFs, VLANs, NAT stages, security controls, and expected evidence only when verified.

### 5. Write safe procedures

Every operational step should include:

- Preconditions.
- Exact target and context.
- Read-only check.
- Action, if any.
- Expected result.
- Failure condition.
- Stop or escalation condition.
- Validation.
- Rollback.

Mark disruptive commands clearly and require approval before execution.

### 6. Add diagrams and tables

Use diagrams for topology and flow. Use tables for:

- Interface and link inventory.
- VLAN and subnet plan.
- VRF and route table mapping.
- Service dependencies.
- Monitoring signals.
- Failure modes.
- Escalation contacts.

Keep diagrams and tables consistent with the verified text.

### 7. Review for operational quality

Check that an operator can answer:

- What is this system?
- What is affected if it fails?
- How do I verify current health?
- What is the safest first action?
- When must I stop?
- How do I roll back?
- Who owns the next decision?

## Runbook output format

Unless another format is requested, return:

1. **Title, owner, scope, and verification date**
2. **Purpose and expected outcome**
3. **Prerequisites and safety notes**
4. **Current-state checks**
5. **Procedure**
6. **Decision points and expected results**
7. **Validation checklist**
8. **Rollback or recovery**
9. **Escalation path**
10. **Evidence and revision history**

## Safety boundary

Do not fill gaps with plausible-looking infrastructure details. Do not publish secrets, private keys, tokens, customer data, or sensitive captures. Do not present an unverified command as production-safe. Clearly distinguish a draft, a reviewed document, and an approved operational runbook.