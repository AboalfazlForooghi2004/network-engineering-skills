# Network Incident Triage

## Purpose

Provide a disciplined, evidence-driven workflow for scoping, diagnosing, communicating, and resolving network incidents across Linux, Cisco, cloud, and hybrid environments.

## Use this skill when

- A service or network path is unavailable, degraded, intermittent, or unexpectedly slow.
- Users report packet loss, latency, timeout, routing failure, DNS failure, or asymmetric traffic.
- A change appears to have caused a network regression.
- Multiple teams need a shared incident timeline and clear ownership.
- The root cause is not yet known and investigation must be structured.

## Core principles

1. Stabilize the incident before optimizing the system.
2. Define the impact and scope before selecting commands.
3. Separate facts, hypotheses, tests, findings, and decisions.
4. Start with the smallest safe observation that can disprove a hypothesis.
5. Compare a failing path with a known-good path whenever possible.
6. Preserve evidence before changing state.
7. Do not restart, flush, reload, fail over, or reconfigure production without explicit approval.
8. Do not infer root cause from a single symptom or a single successful ping.
9. Track the return path, not only the forward path.
10. Record timestamps, affected targets, commands, outputs, and decisions.

## Required inputs

Collect the following information before deep investigation:

- Incident summary in one sentence.
- First observed time and current status.
- Affected users, services, sites, regions, tenants, or environments.
- Source, destination, protocol, port, and expected path.
- Whether the failure is total, partial, intermittent, or performance-related.
- Recent changes, deployments, maintenance, failovers, or topology events.
- One known-good comparison path or target.
- Available telemetry: alerts, logs, counters, captures, flow records, and monitoring graphs.
- Business impact and severity.
- Access limitations and change approval status.

If source, destination, platform, or time window is unknown, ask a focused question instead of guessing.

## Investigation workflow

### 1. Establish the incident boundary

Write down:

- What is failing.
- What is still working.
- Who or what is affected.
- When the behavior started.
- Whether the behavior correlates with a change.

Avoid expanding the scope based on assumptions.

### 2. Check for broad failures first

Use read-only checks to determine whether the issue is local or systemic:

- Interface and link state.
- Device or host reachability.
- Routing and neighbor state.
- DNS resolution.
- Control-plane health.
- Resource saturation.
- Firewall, ACL, NAT, and policy counters.
- Monitoring and alert correlation.

### 3. Trace the path layer by layer

Analyze in order, adapting to the environment:

1. Physical or virtual link state.
2. Layer 2 adjacency, VLAN, trunk, bridge, or bond.
3. ARP, ND, MAC, and neighbor resolution.
4. Local route selection and policy routing.
5. Forwarding, ACL, firewall, NAT, and conntrack.
6. Transport behavior: TCP handshake, retransmission, MTU, MSS, and resets.
7. DNS or application protocol behavior.
8. Return path and state symmetry.

Identify the first point where observed behavior differs from expected behavior.

### 4. Form and test hypotheses

For each hypothesis, record:

- The observed evidence supporting it.
- The evidence that would disprove it.
- The safest test.
- The expected result.
- The actual result.
- The next decision.

Do not run broad or destructive tests when a targeted test can answer the question.

### 5. Compare failing and healthy paths

Compare the smallest meaningful differences:

- Source and destination.
- VLAN, VRF, interface, route table, and next hop.
- ACL or firewall policy.
- NAT behavior.
- MTU and MSS.
- DNS response.
- Device, site, region, or availability zone.
- Time and recent change history.

### 6. Capture evidence when needed

Use targeted observation such as:

- `ping`, `traceroute`, or platform equivalents.
- `ip route get`, `ip rule`, and `ss` on Linux.
- `show interface`, `show arp`, `show mac address-table`, and `show ip route` on Cisco.
- `tcpdump`, Wireshark, SPAN, or embedded packet capture.
- Firewall, NAT, conntrack, flow-log, and load-balancer counters.

Define the capture filter, interface, duration, and stop condition before starting. Avoid collecting unrelated sensitive traffic.

### 7. Stabilize safely

If mitigation is required:

- State the proposed action and expected effect.
- Identify the blast radius and reversibility.
- Take a backup or snapshot where applicable.
- Obtain explicit approval.
- Execute the smallest targeted change.
- Validate immediately.
- Record the exact result and rollback status.

### 8. Confirm recovery

Recovery requires more than one successful command. Confirm:

- The original failing flow works.
- A known-good flow remains healthy.
- The return path is correct.
- Relevant counters and logs are normal.
- Monitoring has recovered.
- No new errors or collateral impact appeared.
- The fix survives the expected control-plane or service behavior.

## Common diagnostic patterns

### Reachability works one way only

Check return routes, policy routing, VRF selection, ACL direction, reverse-path filtering, NAT state, and asymmetric firewall handling.

### TCP fails but ping works

Check destination port listening state, ACL or firewall rules, NAT, application binding, MTU/MSS, SYN/SYN-ACK behavior, and service health.

### Intermittent packet loss

Check interface errors, drops, congestion, ECMP path differences, wireless or virtual link health, CPU saturation, queue drops, and state-table exhaustion.

### DNS fails while direct IP access works

Check resolver configuration, search domains, `ndots`, DNS reachability, UDP/TCP fallback, split-horizon behavior, and policy filtering.

### A recent change is suspected

Compare the pre-change and post-change state, identify the smallest changed object, validate the intended effect, and prepare a targeted rollback rather than reverting unrelated changes.

## Incident output format

Return the investigation in this structure unless the user requests another format:

1. **Impact and scope**
2. **Timeline**
3. **Observed facts**
4. **Current hypotheses**
5. **Tests performed and results**
6. **Most likely fault domain**
7. **Immediate mitigation options**
8. **Validation plan**
9. **Rollback plan**
10. **Root cause or remaining uncertainty**
11. **Follow-up actions and owners**

Clearly label any item that is an assumption or requires confirmation.

## Safety boundary

Operate read-only by default. Do not suggest clearing all counters, flushing firewall or conntrack state, restarting critical services, reloading network devices, changing routes, or failing over production systems without explaining the impact and obtaining explicit confirmation.

Do not request or expose passwords, private keys, tokens, or other credentials. Redact sensitive addresses, identifiers, and payloads when they are not necessary for diagnosis.

Do not declare the incident resolved until the original symptom, a known-good comparison, and relevant monitoring signals have been validated.