# Cloud Network Troubleshooting

## Purpose

Provide provider-aware, evidence-driven guidance for diagnosing AWS, Azure, GCP, and hybrid-cloud networking problems without assuming that provider terminology or behavior is interchangeable.

## Use this skill when

- A cloud workload cannot reach another workload, an on-premises network, the internet, or a managed service.
- VPN, private connectivity, peering, transit routing, NAT, load balancing, DNS, security policy, or endpoint behavior is unexpected.
- A user needs a cloud network design review or a read-only troubleshooting plan.

## Required inputs

Collect:

- Cloud provider and account or subscription context.
- Region, zone, project, VPC/VNet, and tenant boundaries.
- Source and destination resource identifiers.
- Source and destination addresses, protocol, port, and expected path.
- Subnet, route table, security group/firewall, NACL, and endpoint details.
- Connectivity type: internet, peering, transit hub, VPN, dedicated link, or private endpoint.
- DNS name and resolution path where relevant.
- Recent changes, deployment events, and timestamps.
- Available flow logs, health checks, metrics, and provider events.

Do not guess provider, region, account, project, VPC, subnet, or resource identifiers.

## Investigation workflow

### 1. Establish the path model

Write the expected path:

```text
source workload → subnet route table → security policy → transit/peering/VPN/NAT/load balancer → destination policy → destination
return destination → reverse route and policy → source
```

Identify every routing and policy boundary. Cloud connectivity is not proven by the existence of a peering or VPN object.

### 2. Validate resource and subnet state

Check:

- Resource health and network interface attachment.
- Subnet and zone placement.
- Address family and assigned addresses.
- Route table association.
- Network interface security policy.
- Service endpoint or load-balancer target health.
- Provider status and recent events.

### 3. Validate routing

Inspect the relevant route tables and propagation:

- Longest-prefix match.
- Next hop or target.
- Blackhole or inactive routes.
- Transit gateway, virtual hub, or peering attachment.
- VPN tunnel and BGP state.
- Return route.
- Overlapping CIDR conflicts.

Never assume route propagation is enabled or symmetric.

### 4. Validate security controls

Review in order:

- Security groups or stateful workload policies.
- Network ACLs or stateless subnet filters.
- Cloud firewall or centralized inspection policy.
- Load-balancer listener and target policy.
- Host firewall.
- Service-level authorization.

Use logs and counters where available. Distinguish a rejected flow from a missing route and from a destination service that is not listening.

### 5. Validate NAT and egress

For outbound flows, check:

- Private or public address assignment.
- NAT gateway or equivalent placement.
- Route to the NAT resource.
- Return route and ephemeral port capacity.
- Egress security controls.
- Whether the destination expects a stable source address.

### 6. Validate private connectivity

For VPN, dedicated connectivity, peering, transit, or private endpoints, check:

- Attachment state and health.
- Local and remote route advertisements.
- Prefix filters and overlapping ranges.
- Encryption or tunnel status.
- DNS resolution and private hosted zones.
- Endpoint policy and service acceptance.
- Return routing from the service owner.

### 7. Use provider telemetry

Correlate:

- VPC/VNet flow logs.
- Firewall logs.
- Load-balancer access and health logs.
- VPN and BGP logs.
- DNS query logs.
- Cloud monitoring metrics.
- Audit events and deployment history.

Check timestamps and time zones before correlating events.

## Provider-aware rules

- Ask for the provider before using provider-specific commands.
- Use the provider's exact resource names and API terminology.
- Do not treat a security group like a stateless ACL or an NACL like a stateful firewall.
- Treat cloud route propagation, peering, and transit behavior as provider-specific.
- State when a conclusion depends on a provider feature or regional limitation.
- Prefer console/API read operations and exported configuration for evidence.

## Output format

Return:

1. **Provider and scope**
2. **Expected forward and return path**
3. **Resource and subnet checks**
4. **Route analysis**
5. **Security-policy analysis**
6. **NAT, DNS, and private-connectivity checks**
7. **Telemetry to inspect**
8. **Most likely fault domain**
9. **Validation and remediation options**
10. **Risks and open questions**

## Safety boundary

Default to read-only inspection. Do not modify routes, security policies, VPNs, transit attachments, DNS, NAT, or load balancers without explicit approval, a change plan, validation criteria, and rollback. Never request or expose cloud credentials, tokens, or private keys.