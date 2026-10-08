# Projects

Homelab builds, infrastructure case studies, custom tools and proof-of-concept work. Every project has its own folder with a README covering the goal, design, build notes, lessons learned and the skills it demonstrates.

## Structure

```
├── homelab/
│   ├── proxmox-lab-platform/         # The single-node PVE host everything else runs on
│   ├── ludus-cyber-range/            # Reproducible AD attack / detection range
│   ├── proxmox-backup-server/        # Dedicated PBS VM, token ACLs, prune policy
│   ├── docker-services-host/         # Services VM migrated from a Raspberry Pi 5
│   ├── tailscale-remote-access/      # Zero-trust access, MagicDNS, 1Password SSH agent
│   ├── paperclip-ai-agents/          # Self-hosted AI agents with least-privilege lab access
│   ├── minecraft-server/             # systemd-managed game server, Tailscale-only
│   └── pi-travel-router/             # OpenWrt + Tailscale exit-node pocket router
├── infrastructure/
│   ├── network-security-enhancement/ # Professional: segmentation, Fortinet, Veeam/Acronis, PowerShell
│   ├── cloud-vm-management/          # Professional: Azure VM/storage ops, ServiceNow, ITIL
│   └── cisco-enterprise-network-design/  # Multi-VLAN campus with ASA edge, DHCP relay, DNS
├── tools/
│   ├── lab-power-scripts/            # lab-up / lab-down: WoL + Proxmox API orchestration
│   ├── lab-ssh-check/                # Parallel SSH reachability + key-auth checker
│   └── firewall-deadman-switch/      # Timed rollback for remote firewall changes
├── poc/
│   └── mydashboard/                  # Prometheus + Grafana lab health dashboard design
├── CONTRIBUTING.md
└── LICENSE
```

## Project index

### Homelab 🏠

| Project | Summary | Status |
|---|---|---|
| [Proxmox Lab Platform](./homelab/proxmox-lab-platform/) | 16-thread / 96 GB PVE 9 host: storage layout, bridges, guest inventory, research vs ds-lab modes, power management, backups | Active |
| [Ludus Cyber Range](./homelab/ludus-cyber-range/) | Router + Server 2022 DC + Win 11 workstation + Kali, rebuilt from YAML; used for detection engineering and AD attack practice | Active |
| [Proxmox Backup Server](./homelab/proxmox-backup-server/) | PBS 4 on its own datastore disk after a month of silent vzdump failures; token ACL gotchas documented | Active |
| [Docker Services Host](./homelab/docker-services-host/) | NPM, n8n, Grafana, RustDesk stack moving from a Pi 5 to a PVE VM; compose reference included | In progress |
| [Tailscale Remote Access](./homelab/tailscale-remote-access/) | No port-forwards: WireGuard mesh, tailnet names as stable SSH handles, keys in 1Password | Active |
| [Paperclip AI Agents](./homelab/paperclip-ai-agents/) | AI agents that can build and retire VMs through scoped Proxmox/PBS tokens, with protected VMs and a hardened host | Active |
| [Minecraft Server](./homelab/minecraft-server/) | Bare-metal game server under systemd, started/stopped with the lab, Tailscale access | Active |
| [Raspberry Pi Travel Router](./homelab/pi-travel-router/) | OpenWrt pocket router with a Tailscale exit node back to the lab | Build guide |

### Infrastructure 🏗️

| Project | Summary | Status |
|---|---|---|
| [Network Optimisation & Security Enhancement](./infrastructure/network-security-enhancement/) | Assessment, Fortinet firewall/IDS, Veeam/Acronis backup, audits and PowerShell automation across an MSP estate (2022–2025) | Completed |
| [Cloud Services & VM Management](./infrastructure/cloud-vm-management/) | Azure VM/storage/network operations, PowerShell automation, ServiceNow and ITIL (2021–2022) | Completed |
| [Cisco Enterprise Network Design](./infrastructure/cisco-enterprise-network-design/) | VLAN plan, core SVIs and DHCP relay, hardened access ports, ASA NAT/policy, verification commands | Completed |

### Tools 🛠️

| Project | Summary | Status |
|---|---|---|
| [Lab Power Scripts](./tools/lab-power-scripts/) | `lab-up` / `lab-down`: Wake-on-LAN, API polling, mode-aware VM start, graceful shutdown with force-stop fallback, logging | Active |
| [lab-ssh-check](./tools/lab-ssh-check/) | Checks every alias in `~/.ssh/config` in parallel and classifies DOWN / DNS / AUTH / HOSTKEY / HOSTKEY! | Active |
| [Firewall Dead-Man Switch](./tools/firewall-deadman-switch/) | `fw-deadman`: systemd timer that disables the firewall if the change locks you out, plus the procedure | Active |

### Proof of Concept 🧪

| Project | Summary | Status |
|---|---|---|
| [MyDashboard](./poc/mydashboard/) | Prometheus + Grafana lab health dashboard: data sources, panels, and alerts derived from real incidents | Design |

## Conventions

- Real IPs, MACs, usernames, tailnet names and secrets are replaced with documentation placeholders (RFC 5737 `192.0.2.0/24`, `example.ts.net`). VM IDs and hostnames are real.
- Scripts in `tools/` are the versions actually in use, with credentials removed and read from 1Password, the environment, or a 600-mode file instead.
- Professional projects describe how the work was structured and what it delivered; client details are omitted.

See [CONTRIBUTING.md](./CONTRIBUTING.md) for the full sanitisation rules.

---

**Author:** Danny Stanfield · Perth, WA  
**License:** MIT
