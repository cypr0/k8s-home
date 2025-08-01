# ⛵ k8s-home – Personal Kubernetes Cluster

This repository contains the complete Infrastructure-as-Code (IaC) configuration for my home Kubernetes cluster. It is managed with GitOps using [Flux](https://fluxcd.io), [Talos OS](https://www.talos.dev/), and [Helm](https://helm.sh). The cluster is built for security, resilience, and modular deployments, running a wide range of self-hosted services.

---

## 📂 Repository Structure

```
.
├── bootstrap/              # Cluster bootstrap scripts
├── kubernetes/             # Main K8s manifests
│   ├── apps/               # Application and system namespaces
│   ├── components/         # Common Helm repos, SOPS
│   └── flux/               # Flux source definitions and metadata
├── scripts/                # Helper scripts
├── talos/                  # Talos cluster configs and patches
└── templates/              # Templates for GitOps config generation
```

---

## ✨ Features

- **GitOps-first with Flux**: Declarative, version-controlled cluster state.
- **Immutable OS with Talos**: Secure, minimal, and API-driven Kubernetes OS.
- **Helm & App Template**: Most workloads use [`bjw-s/app-template`](https://bjw-s.github.io/helm-charts/docs/app-template/) for consistent configuration.
- **Cilium**: Advanced CNI with eBPF networking, policies, and Gateway API.
- **Ingress via Gateway API**: Uses [Cilium Gateway API](https://docs.cilium.io/en/stable/network/servicemesh/gateway-api/) with `HTTPRoute` resources for L7 routing. TLS termination is handled via cert-manager and Cloudflare Tunnel.
- **Secrets Management**: Uses [`sops`](https://github.com/mozilla/sops) + [`age`](https://github.com/FiloSottile/age) and [External Secrets Operator](https://external-secrets.io) with [1Password Connect](https://1password.com).
- **Monitoring**: Includes [metrics-server](https://github.com/kubernetes-sigs/metrics-server), [reloader](https://github.com/stakater/Reloader), and [spegel](https://github.com/till/spegel) for image mirroring.
- **Storage**: Includes [Longhorn](https://longhorn.io), PostgreSQL via [cloudnative-pg](https://cloudnative-pg.io), and [DragonflyDB](https://www.dragonflydb.io/) as in-memory cache alternative.

---

## 📦 Applications Deployed

### 🔐 Security
- **Authentik** – Identity provider with SSO and LDAP support.
- **External Secrets** – Integrates secrets from 1Password Connect.
- **Cert-Manager** – Automatic TLS with Let's Encrypt and ECDSA support.
- **BSI-compliant TLS** – Certificates configured per [TR-02102-2](https://www.bsi.bund.de/DE/Themen/Standards-und-Zertifizierung/Technische-Richtlinien/TR-nach-Thema-sortiert/tr02102/tr-02102.html).

### 🌐 Networking & Ingress
- **Cilium Gateway API** – Ingress routing is implemented using Gateway API (`Gateway`, `HTTPRoute`) powered by Cilium. This provides a modern, Kubernetes-native way to expose services, avoiding the need for legacy Ingress controllers.
- **Cloudflare Tunnel** – Secures external access to services without exposing public IPs.
- **Cloudflare DNS** – DNS records are automatically managed via the Cloudflare API.
- **k8s-gateway** – Internal DNS resolution for services via CoreDNS.

### 📊 Observability
- **metrics-server** – Resource metrics API for Kubernetes.
- **reloader** – Restarts deployments on config/secret change.
- **spegel** – OCI image mirroring for air-gapped support.

### 📁 Storage & Database
- **Longhorn** – Distributed block storage.
- **cloudnative-pg** – PostgreSQL operator.
- **Dragonfly** – High-performance Redis-compatible cache.

### 🧩 Core Apps
- **Nextcloud** – Personal cloud and productivity suite.
- **Immich** – Self-hosted photo and video backup with AI search.
- **Paperless-ngx** – Document management system.
- **Open WebUI** – Local LLM assistant frontend for inference/control.
- **Echo** – Test application (basic echo service).

---

## 🛡️ Cluster Infrastructure

- **Platform**: Self-hosted on [Netcup](https://www.netcup.de) virtual root servers
- **Nodes**: 3x RS 2000 G11 VPS (8 cores / 16 GB RAM / 512 GB SSD each)
- **Operating System**: [Talos OS](https://www.talos.dev) (immutable, secure-by-design)
- **Firewall**: Dedicated [OPNsense](https://opnsense.org) VM for external routing and VPN
- **Storage Backend**: [TrueNAS SCALE](https://truenas.com/truenas-scale/) providing NFS/block storage

---

## 🔒 Security Practices

- ✅ All apps behind HTTPS ingress
- ✅ TLS via Let's Encrypt (DNS-01)
- ✅ ECDSA certs preferred for stronger crypto
- ✅ Secrets encrypted at rest with `sops`
- ✅ Central identity via Authentik
- ✅ Separation of sensitive namespaces (`security/`, `network/`, etc.)

---

## 🧭 GitOps with Flux

Flux manages all cluster states via:

- **Sources**: Git repositories defined under `kubernetes/flux/`
- **Kustomizations**: Grouped by namespace and component
- **Helm Releases**: Apps are templated with Helm and configured in overlays

---

## 🙏 Acknowledgements

Inspired by:

- [onedr0p/cluster-template](https://github.com/onedr0p/cluster-template) — the gold standard for GitOps home clusters
- [bjw-s/helm-charts](https://github.com/bjw-s/helm-charts) — especially the `app-template`, used for nearly all applications

Thanks to both projects and their communities!

---

## 📄 License

MIT License – see [`LICENSE`](./LICENSE)

---

## 🤝 Contributions

PRs and issues welcome! Feel free to fork and adapt this setup for your own cluster.
