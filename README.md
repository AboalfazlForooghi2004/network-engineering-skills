# Network Engineering Skills

> Practical, precise, and safety-first AI skills for real-world network engineering.

This repository contains focused skills for Linux, Cisco, cloud, Kubernetes, and network operations. Each skill is written to help an AI assistant reason about infrastructure accurately, ask for missing context, and produce implementation-ready guidance without inventing environment details.

## Design principles

- **Read-only by default** — inspect and plan before changing anything.
- **Platform-aware** — do not assume that Linux, IOS-XE, NX-OS, AWS, Azure, and GCP behave the same way.
- **Evidence-driven** — separate observed facts, assumptions, recommendations, and executed changes.
- **Change-safe** — every production change needs scope, validation, backup, approval, and rollback.
- **Secret-safe** — never place credentials, private keys, tokens, or real infrastructure secrets in source files or output.
- **Operationally useful** — provide commands, checks, expected results, failure conditions, and next steps when the required context is available.

## Skill catalog

| Skill | Status | Focus |
|---|---:|---|
| [Network Incident Triage](skills/network-incident-triage/SKILL.md) | Available | Scope, isolate, diagnose, stabilize, and communicate network incidents |
| [Linux Network Troubleshooting](skills/linux-network-troubleshooting/SKILL.md) | Available | Kernel networking, routing, namespaces, nftables, and packet paths |
| [Cisco Network Troubleshooting](skills/cisco-network-troubleshooting/SKILL.md) | Available | IOS, IOS-XE, NX-OS, switching, routing, ACLs, and device health |
| [Routing, VRF, and PBR Design](skills/routing-vrf-pbr-design/SKILL.md) | Available | Routing architecture, policy routing, VRF, ECMP, and return paths |
| [Packet Capture and Flow Analysis](skills/packet-capture-flow-analysis/SKILL.md) | Available | tcpdump, Wireshark, counters, NAT, and flow tracing |
| [Change and Rollback Planning](skills/change-rollback-planning/SKILL.md) | Available | Safe implementation plans, validation, and recovery |
| [Cloud Network Troubleshooting](skills/cloud-network-troubleshooting/SKILL.md) | Available | VPC/VNet routing, security controls, VPN, and private connectivity |
| [Network Documentation and Runbooks](skills/network-documentation-runbook/SKILL.md) | Available | HLD, LLD, diagrams, runbooks, and as-built documentation |
| [Network Automation](skills/network-automation/SKILL.md) | Available | Ansible, Nornir, Netmiko, Scrapli, Terraform, Jinja2, Git, CI/CD, idempotency, dry runs, backup, and rollback |

## Repository structure

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

Open the relevant `SKILL.md` and provide the assistant with the environment facts required by that skill. For network automation, include at least:

- The target environment and change stage.
- Inventory and platform versions.
- The desired end state.
- Scope and constraints.
- The validation plan.
- Backup and rollback requirements.

If any of these are unknown, the skill should ask focused questions instead of guessing.

## Implemented skills

- **Network Incident Triage** — scope, isolate, investigate, stabilize, validate, and document network incidents.
- **Linux Network Troubleshooting** — inspect Linux interfaces, routes, policy routing, namespaces, VRFs, firewall state, conntrack, and packet paths.
- **Cisco Network Troubleshooting** — investigate IOS, IOS-XE, NX-OS, switching, routing, neighbors, policies, and device health.
- **Routing, VRF, and PBR Design** — design forwarding, isolation, policy routing, return paths, failure behavior, and validation matrices.
- **Packet Capture and Flow Analysis** — plan bounded captures, trace packets across capture points, analyze NAT and handshakes, and protect sensitive data.
- **Change and Rollback Planning** — prepare reviewable change plans with backups, staged execution, validation, approval, and targeted rollback.
- **Cloud Network Troubleshooting** — analyze provider-specific routing, security controls, VPN, transit, NAT, DNS, and private connectivity.
- **Network Documentation and Runbooks** — produce accurate HLD, LLD, as-built documentation, and safe operational runbooks.
- **Network Automation** — inventory and platform discovery, tool selection, data modeling, dry-run design, idempotency, progressive execution, validation, backup, rollback, and operational reporting.

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md) and [docs/skill-development-guidelines.md](docs/skill-development-guidelines.md) before opening a pull request.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
