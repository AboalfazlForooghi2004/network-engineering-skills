# Packet Capture and Flow Analysis

## Purpose

Provide a precise workflow for capturing, tracing, and explaining network flows using tcpdump, Wireshark, device capture tools, counters, logs, NAT state, and conntrack evidence.

## Use this skill when

- A packet path, drop point, NAT translation, handshake, or return path must be proven.
- Logs and ping results are insufficient to explain the behavior.
- The user needs a capture filter, capture-point plan, or packet-by-packet interpretation.
- A Linux, Cisco, cloud, container, or firewall flow crosses multiple interfaces or namespaces.

## Required inputs

Collect:

- Source and destination addresses.
- Protocol and ports.
- Expected path and capture points.
- Time window, packet count, and acceptable data sensitivity.
- Interface, namespace, VRF, device, or cloud flow-log source.
- Whether the flow is new, established, failed, or intermittent.
- Whether payload capture is allowed.

Do not capture broad traffic when a narrow filter can answer the question.

## Workflow

### 1. Define the question

State exactly what the capture must prove, such as:

- Does the SYN leave the client?
- Where is the first drop?
- Is DNAT or SNAT applied?
- Does the reply return on the expected path?
- Is the failure caused by MTU, retransmission, reset, or policy?

### 2. Choose capture points

Select the minimum points that distinguish the hypotheses:

- Source ingress.
- Device or namespace ingress.
- Post-policy or post-NAT point.
- Egress toward the destination.
- Destination ingress.
- Return-path ingress and egress.

Document what the packet should look like at each point.

### 3. Build a narrow filter

Examples for Linux:

```bash
tcpdump -ni <interface> -nn -c 100 'host <address> and tcp port <port>'
tcpdump -ni <interface> -nn 'src host <source> and dst host <destination>'
tcpdump -ni <interface> -nn 'tcp[tcpflags] & tcp-syn != 0'
```

Use `-s 0` only when full packet data is required and approved. Prefer headers when payload is unnecessary. Use a bounded count or duration and write captures to a controlled location.

For Cisco or cloud tools, confirm the platform, capture semantics, attachment point, direction, and resource impact before providing commands.

### 4. Record capture metadata

For every capture, record:

- Tool and version if relevant.
- Host, device, namespace, interface, or flow-log source.
- Filter.
- Start and end time.
- Time zone.
- Packet count or file size.
- Snap length and whether payload was captured.
- Any anonymization or redaction.

### 5. Analyze in protocol order

Check:

1. Ethernet, VLAN, and MAC addresses.
2. IP addresses, TTL/hop limit, fragmentation, and DSCP.
3. TCP flags, sequence behavior, window, MSS, retransmissions, resets, and timing.
4. UDP request/response pairing and state behavior.
5. ICMP errors and unreachable messages.
6. DNS transaction IDs, response codes, and transport fallback.
7. TLS or application metadata without exposing payload secrets.
8. NAT changes and connection identity.

### 6. Compare points and directions

Build a timeline showing when the flow appears, disappears, changes address, changes port, or changes metadata. Compare forward and return paths separately. A packet seen on an ingress interface does not prove that it was forwarded.

### 7. Correlate with state and counters

Use firewall counters, NAT state, conntrack entries, interface drops, route lookups, logs, and cloud flow logs. Correlation should explain the packet evidence, not replace it.

### 8. Stop and protect data

Stop captures promptly. Remove or protect files that contain payloads, tokens, cookies, customer data, or internal addresses. Do not commit captures or sensitive logs to Git.

## Interpretation patterns

- SYN with no SYN-ACK: check destination reachability, filtering, listener, return route, and intermediate drops.
- SYN-ACK leaves the server but never reaches the client: check reverse path, NAT, ACL, stateful firewall, and asymmetric routing.
- Repeated retransmissions: check loss, MTU, congestion, queue drops, and receiver behavior.
- ICMP works but TCP fails: check listener, ACL, NAT, service binding, and MSS/MTU.
- Address changes between capture points: identify the exact NAT stage and verify conntrack/state.
- Flow reaches the destination but the client reports failure: inspect application response, TLS, DNS, and return traffic.

## Output format

Return:

1. **Question to prove**
2. **Capture-point diagram**
3. **Exact filters or platform-specific capture plan**
4. **Expected packet sequence**
5. **Observed packet sequence**
6. **First divergence**
7. **Correlated counters and state**
8. **Conclusion and confidence**
9. **Next test or remediation**
10. **Data-handling notes**

## Safety boundary

Use bounded, targeted captures by default. Do not capture or expose sensitive payloads unnecessarily. Do not enable expensive device-wide capture, debug, or packet tracing in production without an impact assessment and explicit approval.