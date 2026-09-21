# Routing, VRF, and PBR Design

## Purpose

Design and review routing architectures involving static routing, dynamic routing, VRFs, policy-based routing, ECMP, multi-path forwarding, segmentation, and symmetric return paths.

## Use this skill when

- Designing a new routed topology or changing an existing one.
- Separating tenants, environments, services, or traffic classes.
- Selecting paths by source, destination, interface, protocol, port, mark, or application class.
- Investigating asymmetric routing, route leaks, blackholes, ECMP behavior, or failover.
- Translating a network design into requirements for a development or automation team.

## Required inputs

Collect:

- Topology and device or host roles.
- All relevant interfaces, addresses, prefixes, VRFs, routing tables, and next hops.
- Forwarding policy and traffic classes.
- Expected forward and return path for each class.
- NAT, firewall, conntrack, and stateful inspection behavior.
- Failure domains and convergence requirements.
- Scale targets: routes, tenants, flows, links, and endpoints.
- Platform and implementation constraints.

Never infer a route, VRF, mark, table ID, or next hop that the user has not supplied.

## Design workflow

### 1. State the problem and invariants

Write the current problem, desired behavior, traffic classes, isolation requirements, and non-negotiable invariants. Examples include:

- No route leak between tenants.
- Symmetric return path for stateful services.
- Only selected traffic uses the alternate link.
- Main routing remains the default for unrelated traffic.
- A failure must converge to a defined alternate path.

### 2. Draw the forwarding model

For each important flow, document:

```text
source → ingress interface → classification → routing table/VRF → next hop → egress → destination
return destination → ingress → state restoration → return table/VRF → egress → source
```

Use separate diagrams or tables for forward and return paths. Do not describe a design as symmetric without validating both directions.

### 3. Choose the control mechanism

Explain whether the requirement is best served by:

- Connected or static routes.
- OSPF, BGP, IS-IS, or another IGP/EGP.
- VRF or routing namespace isolation.
- PBR based on source, destination, interface, protocol, port, or mark.
- ECMP or unequal-cost policy.
- NAT or stateful firewall behavior.

If a classifier is not supported by the native routing policy, identify the required marking, firewall, or application-layer component instead of hiding the limitation.

### 4. Define routing tables and policy precedence

For every policy table, specify:

- Stable table identifier.
- Connected and required destination prefixes.
- Default route behavior.
- Next hop and output interface.
- Source address selection.
- Rule or policy priority.
- Interaction with the main table.
- Behavior when no route matches.

Prefer explicit priorities and documented ownership. Avoid broad defaults in policy tables unless the design explicitly requires them.

### 5. Design isolation and route leaking deliberately

For VRF and multi-tenant designs, document:

- What each VRF can see.
- Which shared services are reachable.
- Whether route leaking is allowed and in which direction.
- Which firewall or policy controls are mandatory.
- How overlapping addresses are distinguished.
- How return traffic is mapped to the correct tenant.

Treat cross-VRF communication as an explicit service policy, not as an accidental consequence of a shared route.

### 6. Analyze stateful behavior

For NAT, firewall, conntrack, and load-balancer flows, verify:

- Which identity is preserved in forward and return directions.
- Where SNAT or DNAT occurs.
- Which table or VRF receives the post-NAT packet.
- Whether a stateful device sees both directions.
- Whether source-port, flow, or conntrack limits can be exhausted.
- Whether asymmetric paths cause drops or state mismatch.

### 7. Define failure behavior

For every primary path, specify:

- Link or neighbor failure signal.
- Detection time.
- Recalculation or failover mechanism.
- New forward and return path.
- Stateful session behavior.
- Recovery and restoration behavior.
- Validation and rollback steps.

### 8. Build a validation matrix

Include positive and negative tests for:

- Each traffic class.
- Each VRF or tenant.
- Shared service access.
- Internet or external access.
- Forbidden cross-tenant traffic.
- Return-path symmetry.
- Link, neighbor, or route failure.
- Overlapping address behavior.
- NAT and policy counters.

## Design output

Return:

1. **Requirements and invariants**
2. **Topology and forwarding diagrams**
3. **Address, VRF, and routing-table model**
4. **Classification and policy rules**
5. **Forward and return flow tables**
6. **NAT, firewall, and state considerations**
7. **Failure and convergence behavior**
8. **Validation matrix**
9. **Implementation handoff**
10. **Risks, limits, and open questions**

## Safety boundary

Do not recommend broad route leaking, global default routes, disabling reverse-path or security controls, or changing production routing without showing the blast radius and rollback plan. Treat route changes and PBR updates as high-impact operations. Never invent topology or production addresses.