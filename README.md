# 👋 Hi, I'm RAK. Herizo - DevSecOps & Infrastructure Engineer

[![RHEL 9 Image Mode](https://img.shields.io/badge/Red%20Hat%20Linux-red?logo=redhat)](https://redhat.com)
[![OpenTofu IaC](https://img.shields.io/badge/Opentofu-gray?logo=opentofu)](https://opentofu.org)
[![Ansible Automation](https://img.shields.io/badge/Ansible-black?logo=ansible)](https://ansible.com)
[![OpenBao SSOT](https://img.shields.io/badge/Openbao-green?logo=openbao)](https://openbao.org)
[![Proxmox VE](https://img.shields.io/badge/Proxmox-gray?logo=proxmox)](https://proxmox.com)
[![GitLab CI/CD](https://img.shields.io/badge/Gitlab-white?logo=gitlab)](https://gitlab.com)


I design and automate **sovereign, self-hosted cloud infrastructures** on **Proxmox VE**. My work focuses on **Infrastructure-as-Code**, **immutable RHEL 9 Linux systems (`bootc`)**, and **Zero-Trust DevSecOps pipelines** where secrets never touch disk and every server lifecycle is reproducible from code.

---

## 🏗️ Featured Architecture: Sovereign Proxmox VE Cloud Platform

My infrastructure portfolio demonstrates **two complementary enterprise deployment paradigms**—a **modular 3-repository pipeline** (used to bootstrap core security infrastructure like **OpenBao**) and a **unified immutable Image Mode factory** (used to deploy self-contained appliances like **Zabbix 7.0 LTS**)—unified by a **secretless OIDC security plane**.

```mermaid
flowchart TB
    subgraph SEC["🛡️ SHARED ZERO-TRUST SECURITY PLANE (SSOT)"]
        BAO["🔐 OpenBao Vault\n(OIDC JWT Federation + Volatile RAM Secrets)"]
    end

    subgraph FLOW1["🔄 PARADIGM 1: Modular Interdependent Pipeline (Core Services e.g. OpenBao)"]
        direction LR
        G1["📀 rhel-golden-image\n(1. Hardened Base Template)"]
        G2["🏗️ pve-infra-iac\n(2. OpenTofu VM Provisioning)"]
        G3["⚙️ pve-apps-configure\n(3. Ansible Day-2 Setup)"]
        G1 ==> G2 ==> G3
    end

    subgraph FLOW2["🚀 PARADIGM 2: Unified Immutable Factory (Appliances e.g. Zabbix)"]
        direction LR
        B1["📦 pve-infra-bootc\n(OCI Container → QCOW2 → OpenTofu + Atomic Updates)"]
    end

    subgraph PVE["🖥️ COMPUTE & STORAGE PLANE — Proxmox VE Cluster"]
        direction LR
        CORE["🔐 Core Security & Cluster Nodes\n(OpenBao, Kubernetes, Management)"]
        APPL["📊 Immutable BootC Appliances\n(Zabbix 7.0 LTS + Persistent XFS)"]
    end

    FLOW1 <-.->|"OIDC JWT & RAM Keys"| SEC
    FLOW2 <-.->|"OIDC JWT & RAM Keys"| SEC
    G3 ==>|"Provisions & Configures"| CORE
    B1 ==>|"Deploys & Auto-Updates"| APPL
```

---

## 🧩 Ecosystem Repositories

### 1. Interdependent 3-Repository Pipeline *(Modular Infrastructure Lifecycle)*
Built for foundational infrastructure and multi-node clusters where image hardening, compute provisioning, and configuration management are cleanly decoupled—**proven in production to bootstrap my central OpenBao secrets cluster**:

| Stage | Repository | Role & Architecture | Core Stack |
| :--- | :--- | :--- | :--- |
| **01** | **[`rhel-golden-image`](https://github.com/herizomarc/rhel-golden-image)** | **Hardened Base Image Factory** | RHEL 9, Packer / Cloud-Image, CIS Hardening, `cloud-init`, `qemu-guest-agent` |
| **02** | **[`pve-infra-iac`](https://github.com/herizomarc/pve-infra-iac)** | **Multi-VM Compute & Network** | OpenTofu, Proxmox VE, GitLab HTTP Remote State, Static IPv4 Allocation |
| **03** | **[`pve-apps-configure`](https://github.com/herizomarc/pve-apps-configure)** | **Idempotent Day-2 Automation** | Ansible, Rootless Podman, In-Memory `ssh-agent`, Service Orchestration |

### 2. Unified Immutable Factory *(RHEL 9 Image Mode / BootC)*
Built for self-contained sovereign appliances where OS packages, custom SELinux policies, and infrastructure definitions live in a **single GitOps repository**—**proven in production to compile and deploy Zabbix 7.0 LTS with atomic pull upgrades**:

| Repository | Role & Architecture | Core Stack |
| :--- | :--- | :--- |
| **[`pve-infra-bootc`](https://github.com/herizomarc/pve-infra-bootc)** | **Unified Immutable OS & IaC Factory** | RHEL 9 `bootc`, Podman, `bootc-image-builder`, OpenTofu, Cloud-Init `scsi2` |

---

## 🏆 Key Production Achievements

* **100% Secretless CI/CD Execution:** Eliminated all static credentials, `.tfvars` secrets, and on-disk SSH keys across 4 repositories by engineering dynamic **GitLab OIDC JWT** federation with **OpenBao** and volatile RAM injection (`/dev/shm` & `ssh-agent`).
* **Zero-Touch Immutable Appliance Delivery:** Engineered an end-to-end **RHEL 9 Image Mode (`bootc`)** factory capable of compiling custom SELinux policies (`krb5_keytab_t`), translating OCI images into UEFI QCOW2 disks, and provisioning self-configuring **Zabbix 7.0 LTS** servers with isolated XFS data volumes (`scsi1`).
* **Atomic OS Lifecycle Management:** Implemented non-destructive, pull-based operating system upgrades (`bootc upgrade --apply`) with instant rollback guarantees while preserving persistent PostgreSQL databases across reboots.
* **Sovereign Enterprise Security & Networking:** Applied **RHCSA** and **CCNA** standards to enforce strict SELinux confinement, VLAN network segmentation, and automated `firewalld` baselines across all virtualized workloads.

---

## 🔭 Engineering Roadmap & Next Projects

Upcoming initiatives currently in design and prototyping to expand the sovereign cloud platform:

| Status | Initiative | Architectural Scope & Target Stack |
| :--- | :--- | :--- |
| 🚧 **In Progress** | **Sovereign Kubernetes Cluster (`k8s-pve-gitops`)** | Multi-node High-Availability Kubernetes deployment provisioned via `pve-infra-iac` with Cilium CNI, MetalLB, and FluxCD / ArgoCD GitOps delivery. |
| 📅 **Planned** | **Full-Stack Observability & Alerting Mesh** | Expanding **Zabbix 7.0 LTS** (`pve-infra-bootc`) with automated host discovery, Prometheus exporters, and Grafana dashboards across all Proxmox nodes. |
| 📅 **Planned** | **Automated PKI & Short-Lived SSH Certificates** | Leveraging **OpenBao PKI & SSH Secrets Engines** to replace static Ed25519 keys with dynamic, 1-hour signed SSH certificates and internal TLS automation. |

---

## 🎓 Certifications & Core Domains

[![Red Hat RHCSA](https://img.shields.io/badge/Red%20Hat-RHCSA-red?logo=redhat)](https://www.credly.com/badges/2c963e81-b421-44bd-9d4e-56c0c6f61be1/public_url)
[![Cisco CCNA v7_ITN](https://img.shields.io/badge/Cisco-ITN-white?logo=cisco)](https://www.credly.com/badges/c4169874-c332-4ef4-8e8b-96f64c60d56c/public_url)
[![Cisco CCNA v7_SRWE](https://img.shields.io/badge/Cisco-SRWE-white?logo=cisco)](https://www.credly.com/badges/d39f6e2e-0917-459b-8df6-1b83a22d64b2)
[![Cisco CCNA v7_ENSA](https://img.shields.io/badge/Cisco-ENSA-white?logo=cisco)](https://www.credly.com/badges/1b0f5a11-b909-451e-9040-5f5c627bb5af)

* **Red Hat Certified System Administrator (RHCSA)** — Enterprise Linux administration, SELinux enforcement, storage management (LVM/XFS), systemd, and container operations.
* **Cisco Certified Network Associate (CCNA v7 — ITN, SRWE, ENSA)** — Enterprise routing & switching, VLAN segmentation, OSPF, ACLs, NAT, and network security fundamentals.
