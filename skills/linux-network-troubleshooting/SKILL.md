# Linux Network Troubleshooting

## Purpose

Provide a safe, evidence-driven workflow for diagnosing Linux networking problems across hosts, containers, network namespaces, VRFs, bridges, VLANs, routing policy, firewalls, NAT, and sockets.

## Use this skill when

- A Linux host cannot reach a destination or receives unexpected traffic.
- A service is listening but clients cannot connect.
- Routing, policy routing, VRF, namespace, bridge, VLAN, or veth behavior is unclear.
- nftables, iptables, conntrack, NAT, reverse-path filtering, MTU, or kernel forwarding may be involved.
- The user needs a read-only diagnostic plan or an implementation-ready remediation plan.

## Required inputs

Collect:

- Distribution, kernel version, network manager, and relevant service versions.
- Host, namespace, VRF, container, or interface under investigation.
- Source, destination, protocol, port, and expected path.
- Current symptoms, timestamps, and whether the issue is total or intermittent.
- Relevant topology, addresses, routes, and recent changes.
- Whether the request is read-only, lab, staging, or production.

Do not guess interface names, namespace names, addresses, routing tables, or firewall backends.

## Diagnostic workflow

### 1. Establish link and address state

Start with read-only checks:

```bash
ip -br link
ip -br addr
ip -d link show
ethtool <interface>
cat /sys/class/net/<interface>/operstate
cat /sys/class/net/<interface>/statistics/{rx_errors,rx_dropped,tx_errors,tx_dropped}
```

Check carrier, administrative state, MTU, VLAN or bond membership, addresses, duplicate addresses, and error counters.

### 2. Determine the routing decision

Inspect both ordinary and policy routing:

```bash
ip route show table main
ip route show table all
ip rule show
ip route get <destination> from <source> iif <interface>
```

Explain the selected table, next hop, output interface, source address, metric, and whether a rule or VRF changed the result. Always check the return path separately.

### 3. Check neighbors and Layer 2

Use targeted checks:

```bash
ip neigh show
bridge link show
bridge vlan show
bridge fdb show
```

For namespaces or VRFs, run the equivalent command in the correct context. Distinguish ARP/ND failure from routing failure and from firewall filtering.

### 4. Check sockets and local services

```bash
ss -s
ss -lntup
ss -tanp
systemctl status <service>
```

Confirm whether the service is listening on the expected address and port, whether it is bound only to loopback, and whether established connections show retransmits or resets.

### 5. Check namespaces, VRFs, bridges, and veth pairs

```bash
ip netns list
ip link show type vrf
ip link show type bridge
ip -d link show type vlan
```

Use `ip netns exec <namespace> ...` only after confirming the namespace. Trace the complete path across veth peers, bridges, VRF tables, and namespace boundaries. Do not assume that the root namespace and a child namespace share routes, firewall state, or sysctl values.

### 6. Check forwarding, reverse-path filtering, and kernel state

Inspect only relevant values:

```bash
sysctl net.ipv4.ip_forward
sysctl net.ipv4.conf.all.rp_filter
sysctl net.ipv4.conf.default.rp_filter
sysctl net.ipv4.conf.<interface>.rp_filter
sysctl net.ipv4.conf.all.accept_local
sysctl net.ipv4.conf.all.arp_ignore
sysctl net.ipv4.conf.all.arp_announce
```

Explain how strict or loose reverse-path filtering, forwarding, ARP behavior, and namespace-specific sysctls affect the observed flow. Do not recommend disabling controls globally without explaining the security and routing consequences.

### 7. Inspect firewall, NAT, and conntrack

First identify the active backend:

```bash
nft list ruleset
iptables -S
iptables -t nat -S
conntrack -S
```

Use counters, hook, priority, interface, address, port, connection state, mark, and NAT translation to locate the first drop or unexpected transformation. Do not flush rules or conntrack state as a diagnostic shortcut.

### 8. Capture the flow

Use a narrow capture at a known point:

```bash
tcpdump -ni <interface> -nn -c <count> '<filter>'
```

Capture at multiple points only when needed to distinguish ingress, forwarding, NAT, and egress. Record the exact interface, namespace, filter, duration, and expected result.

## Common interpretations

- A route in `main` does not prove that an `ip rule` or VRF will use it.
- A listening socket does not prove that a firewall, namespace route, or return path permits the flow.
- `rp_filter` can drop a packet when the reverse lookup selects another interface.
- NAT depends on conntrack state and can fail through exhaustion or tuple/port limits.
- A bridge can forward frames without the host having an IP on that bridge.
- A veth pair connects namespaces but does not create routes automatically.
- A successful local ping may not validate the application path, port, NAT, or return direction.

## Output format

Return:

1. **Scope and assumptions**
2. **Observed facts**
3. **Commands to run, with the reason for each**
4. **Expected evidence**
5. **Likely fault domains**
6. **Remediation options**
7. **Validation checks**
8. **Rollback and safety notes**
9. **Open questions**

## Safety boundary

Operate read-only by default. Do not flush firewall or conntrack state, delete routes, change sysctls, restart networking, or modify interfaces without explicit approval and a rollback plan. Never request or expose credentials or private keys.