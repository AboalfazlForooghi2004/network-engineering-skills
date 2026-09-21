<div align="center">

# Network Engineering Skills

### Practical AI guidance for Linux, Cisco, Cloud, and Network Operations

<p>
  <a href="https://github.com/AboalfazlForooghi2004/network-engineering-skills/stargazers"><img src="https://img.shields.io/github/stars/AboalfazlForooghi2004/network-engineering-skills?style=for-the-badge&logo=github&color=24292f" alt="GitHub stars"></a>
  <a href="https://github.com/AboalfazlForooghi2004/network-engineering-skills"><img src="https://img.shields.io/badge/skills-9-0A7EA4?style=for-the-badge" alt="Nine skills"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-2ea44f?style=for-the-badge" alt="MIT License"></a>
  <a href="docs/safety-and-change-policy.md"><img src="https://img.shields.io/badge/default-read--only-e05d44?style=for-the-badge" alt="Read-only by default"></a>
</p>

<p>
  <strong>Precise.</strong> <strong>Operational.</strong> <strong>Safety-first.</strong>
</p>

</div>

---

## What this is

`network-engineering-skills` is a collection of focused AI skills for real infrastructure work. Each skill helps an assistant understand the environment, ask for missing facts, reason about the packet or change path, and produce guidance that an engineer can review and use.

The goal is not to generate random command lists. The goal is to turn an ambiguous network request into a structured, evidence-based engineering workflow.

> **Core rule:** inspect first, plan clearly, validate everything, and never invent infrastructure facts.

## Why these skills exist

Network engineering crosses multiple layers at once:

- Linux kernel networking, namespaces, VRFs, bridges, nftables, and conntrack
- Cisco switching, routing, control plane, and platform-specific behavior
- Cloud routing, security policy, VPN, transit, NAT, DNS, and private connectivity
- Automation, configuration management, idempotency, dry runs, and rollback
- Incident response, packet analysis, change management, and documentation

These skills keep those workflows focused instead of forcing every task through one oversized prompt.

## Skill library

### Diagnose

| Skill | What it helps with |
|---|---|
| [Network Incident Triage](skills/network-incident-triage/SKILL.md) | Scope incidents, test hypotheses, isolate fault domains, stabilize service, and document recovery |
| [Linux Network Troubleshooting](skills/linux-network-troubleshooting/SKILL.md) | Investigate interfaces, routes, policy routing, namespaces, VRFs, firewall state, conntrack, and sockets |
| [Cisco Network Troubleshooting](skills/cisco-network-troubleshooting/SKILL.md) | Troubleshoot IOS, IOS-XE, NX-OS, switching, routing, neighbors, ACLs, and device health |
| [Packet Capture and Flow Analysis](skills/packet-capture-flow-analysis/SKILL.md) | Plan bounded captures and trace handshakes, NAT, drops, retransmissions, and return paths |

### Design and change

| Skill | What it helps with |
|---|---|
| [Routing, VRF, and PBR Design](skills/routing-vrf-pbr-design/SKILL.md) | Design forwarding, segmentation, policy routing, ECMP, symmetric paths, route leaking, and failover |
| [Change and Rollback Planning](skills/change-rollback-planning/SKILL.md) | Prepare reviewable changes with pre-checks, backups, staged execution, validation, approval, and rollback |
| [Cloud Network Troubleshooting](skills/cloud-network-troubleshooting/SKILL.md) | Analyze VPC/VNet routing, security controls, VPN, transit, NAT, DNS, load balancers, and private endpoints |

### Build and operate

| Skill | What it helps with |
|---|---|
| [Network Automation](skills/network-automation/SKILL.md) | Use Ansible, Nornir, Netmiko, Scrapli, Terraform, Jinja2, Git, and CI/CD safely |
| [Network Documentation and Runbooks](skills/network-documentation-runbook/SKILL.md) | Produce HLD, LLD, as-built documents, topology notes, runbooks, and operational procedures |

## How every skill behaves

```text
Request
  ↓
Understand scope and environment
  ↓
Ask for missing facts
  ↓
Separate facts from assumptions
  ↓
Inspect or design the expected path
  ↓
Propose a safe, reviewable workflow
  ↓
Validate with evidence
  ↓
Document results, risks, and rollback
```

### Shared quality bar

- **Read-only by default** — inspect before changing.
- **Platform-aware** — do not mix Linux, IOS-XE, NX-OS, AWS, Azure, or GCP behavior without verification.
- **Evidence-driven** — distinguish observed facts, hypotheses, recommendations, and executed actions.
- **Change-safe** — define scope, impact, validation, backup, approval, and rollback.
- **Idempotent where possible** — repeated runs must not create duplicate or unexpected state.
- **Secret-safe** — never include passwords, tokens, private keys, or real credentials.
- **Honest about uncertainty** — label unknown values instead of filling them with plausible guesses.

## Example requests

```text
A Linux host can reach the gateway but not the application subnet.
Trace the route, policy routing, firewall, conntrack, and return path using read-only checks.
```

```text
Design a VRF and PBR architecture for two traffic classes over two uplinks.
Show forward and return paths, failure behavior, validation tests, and rollback risks.
```

```text
Create an Ansible workflow to configure Cisco access switches.
Start with inventory and platform questions, then produce a dry-run, backup, validation, and rollback plan.
```

```text
A cloud workload cannot reach an on-premises service over VPN.
Analyze route propagation, security controls, DNS, tunnel state, and the return path.
```

## Repository layout

```text
.
├── docs/
│   ├── safety-and-change-policy.md
│   └── skill-development-guidelines.md
├── skills/
│   ├── change-rollback-planning/
│   │   └── SKILL.md
│   ├── cisco-network-troubleshooting/
│   │   └── SKILL.md
│   ├── cloud-network-troubleshooting/
│   │   └── SKILL.md
│   ├── linux-network-troubleshooting/
│   │   └── SKILL.md
│   ├── network-automation/
│   │   └── SKILL.md
│   ├── network-documentation-runbook/
│   │   └── SKILL.md
│   ├── network-incident-triage/
│   │   └── SKILL.md
│   ├── packet-capture-flow-analysis/
│   │   └── SKILL.md
│   └── routing-vrf-pbr-design/
│       └── SKILL.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## Using a skill

1. Open the relevant `SKILL.md`.
2. Provide the assistant with the platform, topology, scope, and desired outcome.
3. Include the current state and any known constraints.
4. Ask for a read-only assessment or a change plan first.
5. Review the proposed validation and rollback before execution.
6. Record the evidence and outcome after the work.

If important context is missing, the skill should ask focused questions instead of guessing.

## Safety and scope

These skills are designed to assist engineering decisions, not to bypass change control.

Before a production-impacting action, the workflow must include:

- Target and scope confirmation
- Expected impact and blast radius
- Backup or snapshot
- Validation criteria
- Rollback procedure
- Explicit approval

Read the full [Safety and Change Policy](docs/safety-and-change-policy.md) for the operating boundary.

## Contributing

Contributions are welcome when they improve technical accuracy, operational safety, clarity, or reuse.

Before opening a pull request:

- Read [CONTRIBUTING.md](CONTRIBUTING.md).
- Follow the [Skill Development Guidelines](docs/skill-development-guidelines.md).
- Keep examples free of secrets and real credentials.
- Add validation, failure handling, and rollback guidance.
- Preserve the read-only-by-default behavior.

## License

This project is licensed under the [MIT License](LICENSE).

<div align="center">

<sub>Built for engineers who care about the packet path, the return path, and the rollback path.</sub>

</div>
