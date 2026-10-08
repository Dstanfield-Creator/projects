# MyDashboard — Lab Monitoring & Analytics

> One screen that answers "is the lab healthy?": hypervisor and VM metrics, backup status, container state, tailnet reachability and router health, with alerts for the things that have actually broken before.

**Category:** POC · **Status:** Design and build in progress · **Repo:** [MyDashboard](https://github.com/Dstanfield-Creator/MyDashboard) (private) · **Stack:** Prometheus + Grafana on the [Docker Services Host](../../homelab/docker-services-host/)

## Why

Every lab failure so far was visible in some log nobody was reading: a backup disk filling for a month, a game server unreachable overnight, a retired host still being probed. The dashboard exists to surface exactly those signals.

## Data sources

| Signal | Exporter / source | Scrape target |
|---|---|---|
| Proxmox node CPU/RAM/storage, per-VM state and resource use | `prometheus-pve-exporter` with a read-only `PVEAuditor` token | hypervisor API |
| Backup job success, datastore usage, last-verified | PBS built-in `/metrics` and the PVE task API | PBS :8007 |
| Container state, restarts, resource use | `cAdvisor` | Docker host |
| Host-level metrics on every Linux guest | `node_exporter` | each VM |
| Reachability of every SSH alias | `blackbox_exporter` TCP probes, mirroring `lab-ssh-check` | lab hosts |
| Tailnet node online/offline | `tailscale status --json` via a small textfile-collector script | workstation |
| Router WAN/LAN throughput, client count | UniFi Poller (`unpoller`) | UniFi controller |
| Minecraft players online, TPS | `mc-monitor` exporter | game host |

## Panels (first cut)

1. **Lab status strip:** hypervisor up/down, mode (research / ds-lab), number of running VMs, last `lab-up` time.
2. **Storage:** `local` and `local-lvm` usage with a 85% threshold line, PBS datastore usage and dedup factor.
3. **Backups:** last 8 weekly jobs as green/red tiles, age of the newest backup per VM.
4. **Compute:** node CPU/RAM, top-5 VMs by RAM.
5. **Services:** container table (state, restarts last 24 h), NPM certificate expiry.
6. **Reachability:** every SSH alias as a tile, matching `lab-ssh-check` statuses.
7. **Network:** WAN throughput, client count, tailnet nodes online.
8. **Game server:** players online, TPS, last backup age.

## Alerts (Alertmanager → n8n → phone)

| Rule | Why it exists |
|---|---|
| `local` storage > 85% for 1 h | the backup disk-full incident |
| Any VM backup older than 8 days | the month of silent failures |
| Minecraft host unreachable while hypervisor is up | the overnight outage |
| Container restart count > 3 in 1 h | crash loops |
| Any blackbox probe failing for 10 min that is **not** in the skip list | mirrors `lab-ssh-check` semantics |

## Build plan

- [x] Define sources and panels (this document)
- [ ] Deploy Prometheus, Alertmanager, Grafana via Compose on the Docker host
- [ ] `prometheus-pve-exporter` with a scoped token; dashboards for node + VMs
- [ ] PBS metrics and backup-age rule
- [ ] blackbox probes seeded from `~/.ssh/config` (same awk as `lab-ssh-check`)
- [ ] n8n flow for Alertmanager webhooks → push notification
- [ ] Publish sanitised dashboard JSON to the MyDashboard repo

## Design decisions

- **Prometheus over an agent-based SaaS.** Everything stays in the lab; nothing phones home.
- **Read-only tokens everywhere.** The exporter can see, not act.
- **Alert on history, not hypotheticals.** Every rule maps to an incident that already happened.

## Skills demonstrated

Observability design · Prometheus / Grafana · exporter selection and scoping · alert design from incident history

## Related

- [Docker Services Host](../../homelab/docker-services-host/) · [Proxmox Backup Server](../../homelab/proxmox-backup-server/) · [lab-ssh-check](../../tools/lab-ssh-check/)

---

**Author:** Danny Stanfield · Perth, WA  
**License:** MIT
