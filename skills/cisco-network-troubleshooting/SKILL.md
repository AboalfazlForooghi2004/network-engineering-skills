# Cisco Network Troubleshooting

## Purpose

Provide platform-aware, read-only-first guidance for diagnosing Cisco switching, routing, interface, control-plane, ACL, NAT, and high-availability problems.

## Use this skill when

- A Cisco device has reachability, routing, switching, neighbor, or performance problems.
- OSPF, BGP, HSRP, VRRP, STP, EtherChannel, VLAN, ACL, NAT, or QoS behavior is unexpected.
- The user needs a show-command plan, configuration review, change plan, or rollback plan.
- A Cisco device must be compared with a known-good peer or site.

## Required inputs

Ask for or identify:

- Hardware or virtual platform.
- OS family and exact version: IOS, IOS-XE, NX-OS, ASA, or another platform.
- Device role and topology position.
- Source, destination, protocol, port, VRF, VLAN, and expected path.
- Relevant interface, neighbor, route, policy, and recent-change details.
- Whether the device is lab, staging, or production.

Do not provide platform-specific commands until the platform and version are known. Never assume that an IOS command has the same syntax or effect on NX-OS or ASA.

## Investigation workflow

### 1. Check device and interface health

Use read-only commands appropriate to the platform, such as:

```text
show version
show inventory
show processes cpu sorted
show processes memory sorted
show interfaces status
show interfaces counters errors
show interfaces <interface>
show logging
```

Look for link flaps, CRC errors, input/output drops, duplex or speed mismatch, queue drops, MTU mismatch, high CPU, memory pressure, and recent reloads.

### 2. Validate Layer 2

For switching issues, inspect:

```text
show vlan brief
show interfaces trunk
show spanning-tree summary
show spanning-tree vlan <vlan-id>
show etherchannel summary
show mac address-table dynamic
show interfaces switchport
```

Confirm VLAN existence, trunk allowance, native VLAN consistency, port mode, STP state, EtherChannel consistency, MAC learning, and possible loops. Do not recommend disabling STP as a general troubleshooting step.

### 3. Validate Layer 3 and VRF context

Use:

```text
show ip interface brief
show ip route
show ip route vrf <vrf-name>
show arp
show ip cef <destination> detail
show vrf
```

Explain the selected route, administrative distance, metric, next hop, outgoing interface, VRF, recursive resolution, and return path. A route in the global table does not prove that the target VRF has a usable route.

### 4. Validate routing neighbors

For OSPF, BGP, or other protocols, identify the exact platform syntax and inspect:

- Neighbor state and uptime.
- Local and remote addresses.
- VRF and update source.
- Authentication and timers.
- Prefix count and policy.
- Route filtering, redistribution, and next-hop handling.
- Interface or transport reachability.

Do not recommend clearing a neighbor or resetting a process until the impact, expected convergence, and rollback are stated and the user approves it.

### 5. Validate policy and filtering

Review relevant ACL, NAT, QoS, and control-plane policy entries. Use counters and hit counts where available. Check direction and attachment point, not just the text of the rule. Distinguish an ACL drop from a route failure and from an application refusal.

### 6. Validate high availability

For HSRP, VRRP, StackWise, vPC, MLAG, or similar systems, check active/standby state, priority, tracking, timers, peer health, consistency, and split-brain indicators. Test both forward and return paths without forcing a failover unless explicitly approved.

### 7. Compare with a healthy peer

Compare platform, interface, VLAN, VRF, route, neighbor, policy, and recent-change state. Report only meaningful differences; do not copy a peer configuration blindly.

## Command handling rules

- Label every command as read-only, configuration-changing, disruptive, or destructive.
- Use placeholders for sensitive or environment-specific values.
- Provide one command family at a time and explain the expected output.
- If output is unavailable, state what would confirm or disprove the hypothesis.
- Do not invent interface names, VLANs, route entries, or neighbor addresses.

## Output format

Return:

1. **Platform and scope**
2. **Impact and expected behavior**
3. **Read-only checks**
4. **Interpretation of each result**
5. **Most likely fault domain**
6. **Corrective options**
7. **Validation plan**
8. **Change and rollback plan, if needed**
9. **Open questions**

## Safety boundary

Default to show and inspection commands. Do not reload devices, clear counters, clear sessions, reset routing processes, remove ACLs, change routes, or modify interfaces without explicit approval, an impact statement, validation criteria, and rollback steps. Never include credentials, private keys, or enable secrets.