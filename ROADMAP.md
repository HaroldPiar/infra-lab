# infra-lab

A hands-on course to learn infrastructure and networking fundamentals, using Docker as the lab environment. It goes from the basics to advanced topics, one module at a time, and everything is built in public.

> **Goal:** understand how networks and infrastructure really work by building, breaking and fixing them.

## How this repo works

- Every module lives in its own folder (`00-environment/`, `01-docker-linux/`, ...).
- Each folder contains a `docker-compose.yml` (or lab files) and a `README.md` with:
  - what I built,
  - what I learned, in my own words,
  - what I broke on purpose and how I diagnosed it.
- Everything is defined as code. If I did something by hand, it is not finished until it is in a file.

## Environment

- Windows + Docker Desktop (WSL2 backend) + Ubuntu on WSL2
- VS Code with the WSL extension
- Work inside the WSL filesystem (not under `C:\`) for better performance
- Main diagnostic tool: [`nicolaka/netshoot`](https://github.com/nicolaka/netshoot)

## Known limits of Docker as a lab

- Containers share the host kernel, so there is no real Layer 2 (VLANs, STP, switches).
- `macvlan` and broadcast-heavy services (DHCP) do not behave like a real network on Docker Desktop, because it runs inside a VM.
- Windows Server and Active Directory cannot run in Docker (Samba AD is the closest option); that would need VMs.
- For what Docker cannot do, Module 11 uses **containerlab**.

## Progress

| # | Module | Level | Status |
|---|--------|-------|--------|
| 0 | Environment setup | Foundations | ⬜ |
| 1 | Docker internals and Linux basics | Foundations | ⬜ |
| 2 | Networking fundamentals | Networking | ⬜ |
| 3 | Routing | Networking | ⬜ |
| 4 | Firewall and NAT | Networking | ⬜ |
| 5 | DNS | Services | ⬜ |
| 6 | HTTP, reverse proxy and TLS | Services | ⬜ |
| 7 | Infrastructure services | Services | ⬜ |
| 8 | VPN | Advanced | ⬜ |
| 9 | Observability | Advanced | ⬜ |
| 10 | Automation and IaC | Advanced | ⬜ |
| 11 | Advanced networking and resilience | Advanced | ⬜ |

Suggested pace: one module per week, 2 to 3 hours of practice each.

---

## Level 1: Foundations

### Module 0: Environment setup

**Objective:** have a working, reproducible lab environment.

- [ ] Confirm Docker Desktop uses the WSL2 engine
- [ ] Install Ubuntu on WSL2 and clone this repo inside it
- [ ] Learn the basics: `docker run`, `exec`, `logs`, `ps`, `docker compose up/down`
- [ ] Run a `netshoot` container and open a shell in it

**Deliverable:** a `docker-compose.yml` with a `netshoot` service and a README documenting the setup.

### Module 1: Docker internals and Linux basics

**Objective:** understand what a container really is and learn the Linux tools used to inspect it.

- [ ] Images, layers, volumes, environment variables
- [ ] Default Docker networks (`bridge`, `host`, `none`)
- [ ] Diagnostic commands: `ip a`, `ip route`, `ss -tulpn`, `ps`, `journalctl`

**Lab:** run nginx and PostgreSQL, enter them with `exec`, and list their processes, ports and interfaces.

**Deliverable:** a README explaining what a container is (namespaces, cgroups) in my own words.

---

## Level 2: Networking

### Module 2: Networking fundamentals

**Objective:** understand IP addressing and how traffic moves inside a subnet.

- [ ] TCP/IP model, IP addresses, subnet masks, CIDR, gateways, ports
- [ ] ARP and how hosts find each other
- [ ] Custom Docker bridge networks with fixed subnets

**Lab:** two containers on a custom network. Capture ARP, ICMP and the TCP handshake with `tcpdump`.

**Deliverable:** annotated packet captures and a subnetting cheat sheet.

### Module 3: Routing

**Objective:** connect two networks through a router.

```
[client] ── net_a (10.10.1.0/24) ── [router] ── net_b (10.10.2.0/24) ── [web server]
```

- [ ] Router container with two interfaces and `ip_forward` enabled
- [ ] Static routes on client and server
- [ ] `traceroute` to see the extra hop

**Deliverable:** a Compose file for the topology and a diagram in the README.

### Module 4: Firewall and NAT

**Objective:** control and translate traffic.

- [ ] `iptables` or `nftables`: INPUT, OUTPUT, FORWARD chains
- [ ] Masquerade (source NAT) and port forwarding (destination NAT)
- [ ] Stateful rules (`conntrack`)

**Lab:** block and allow traffic between both networks, and document every rule.

**Deliverable:** a rules file with a comment explaining each rule, plus tests that prove they work.

---

## Level 3: Services

### Module 5: DNS

**Objective:** understand name resolution end to end.

- [ ] Record types (A, AAAA, CNAME, MX, TXT), zones, resolvers, TTL
- [ ] Own DNS server (CoreDNS or BIND) with a `lab.local` zone
- [ ] Query and debug with `dig`

**Deliverable:** a working internal zone and a write-up of a resolution I traced step by step.

### Module 6: HTTP, reverse proxy and TLS

**Objective:** publish several services behind a single entry point, securely.

- [ ] Nginx, Caddy or Traefik as reverse proxy
- [ ] Own certificate authority and HTTPS
- [ ] Host-based routing

**Lab:** `app1.lab.local` and `app2.lab.local` behind one proxy with HTTPS.

**Deliverable:** the stack in Compose plus instructions to trust the lab CA.

### Module 7: Infrastructure services

**Objective:** build the services a small office network needs.

- [ ] SSH with keys and a bastion host
- [ ] File sharing (Samba or NFS)
- [ ] DHCP and NTP (mind the Docker limitations)

**Deliverable:** a "mini office network" Compose project with a diagram.

---

## Level 4: Advanced

### Module 8: VPN

**Objective:** connect two private networks securely.

- [ ] WireGuard between two lab networks
- [ ] Key management, allowed IPs, routing through the tunnel

**Deliverable:** a working site-to-site tunnel and a README explaining the handshake and routing.

### Module 9: Observability

**Objective:** see what the infrastructure is doing.

- [ ] Prometheus and Grafana for metrics
- [ ] Loki for logs
- [ ] A basic alert

**Deliverable:** a dashboard exported as code and one alert that fires on purpose.

### Module 10: Automation and Infrastructure as Code

**Objective:** make the whole lab reproducible with one command.

- [ ] Advanced Compose: profiles, healthchecks, multiple networks
- [ ] `Makefile` shortcuts
- [ ] Ansible to configure containers
- [ ] Terraform with the Docker provider
- [ ] CI (GitHub Actions) to validate the Compose files

**Deliverable:** `make up` builds the lab and CI validates it.

### Module 11: Advanced networking and resilience

**Objective:** go beyond what plain Docker can do.

- [ ] **containerlab** with FRRouting: VLANs, OSPF, BGP
- [ ] HAProxy + keepalived for high availability
- [ ] Optional: local Kubernetes with `kind`

**Deliverable:** a multi-router topology with dynamic routing and a failover test.

---

## Ground rules

1. Everything is defined in files, no manual-only setups.
2. Every module ends with a README in my own words.
3. Every lab includes a "break it and diagnose it" part: I turn something off on purpose and fix it using the tools.
4. Commit small and often; the commit history is part of the learning log.
