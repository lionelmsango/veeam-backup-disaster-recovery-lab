# Veeam Backup & Replication Lab - Implementation Journey

> *Enterprise backup infrastructure deployment using Veeam Community Edition - A learning experience in backup architecture and troubleshooting*

---

## 📋 Project Overview

**Objective:** Implement Veeam Backup & Replication infrastructure to protect Windows and Linux VMs with automated backup jobs and disaster recovery capabilities.

**Environment:** VMware Workstation  
**Edition:** Veeam Backup & Replication 13.0.1 Community Edition (Free)  
**Current Status:** Infrastructure deployed, backup jobs configured, troubleshooting backup execution issues

## What I Built (So Far)

This lab demonstrates the foundation of a complete backup infrastructure:

### ✅ Successfully Completed

- **Infrastructure Planning:** Designed network architecture with dedicated backup server
- **Storage Configuration:** Created dedicated 199 GB backup repository partition
- **Veeam Installation:** Full installation of Veeam Backup & Replication 13
- **Database Setup:** PostgreSQL database backend configured
- **Repository Creation:** Backup storage location configured and verified
- **Backup Jobs:** Two automated backup jobs created (Windows 11, Ubuntu)
- **Agent Deployment:** Veeam agents deployed to target VMs
- **Job Scheduling:** Daily backup schedules configured

### ⚠️ Currently Troubleshooting

---

## The Architecture

```
┌──────────────────────────────────────────────────────┐
│      Virtual Network (192.168.10.0/24)               │
├──────────────────────────────────────────────────────┤
│                                                       │
│  ┌─────────────────────────────────────┐             │
│  │   Veeam Backup Server               │             │
│  │   Windows Server 2022               │             │
│  │   IP: 192.168.10.10                 │             │
│  │                                     │             │
│  │   Components:                       │             │
│  │   ✓ Veeam Backup & Replication 13   │             │
│  │   ✓ PostgreSQL 13 Database          │             │
│  │   ✓ Backup Repository (R:\)         │             │
│  │   ✓ Capacity: 199 GB                │             │
│  └──────────┬──────────────────────────┘             │
│             │                                         │
│             │ Agent-Based Backups                     │
│             │                                         │
│     ┌───────┴─────────┬──────────────┐               │
│     │                 │              │               │
│     ▼                 ▼              ▼               │
│  ┌──────┐         ┌──────┐      ┌──────┐            │
│  │Win11 │         │Ubuntu│      │Future│            │
│  │.10.20│         │.10.30│      │ VMs  │            │
│  └──────┘         └──────┘      └──────┘            │
│                                                       │
└──────────────────────────────────────────────────────┘
```

### Phase 1: Storage Preparation

**Repository Disk Partition Created**

![Repository Disk](https://github.com/lionelmsango/veeam-backup-disaster-recovery-lab/blob/addcbf92b0385f7b4faacb6f94affd772abad069/screenshots/1__repository_disk_partition_created.jpg.jpg)
*Created dedicated 199 GB partition (R:) for Veeam backup repository - separate from OS disk for better I/O performance and storage management.*

---

### Phase 2: Veeam Installation

**Installation Splash Screen**

![Veeam Installation](https://github.com/lionelmsango/veeam-backup-disaster-recovery-lab/blob/addcbf92b0385f7b4faacb6f94affd772abad069/screenshots/2_veeam_installation_splash_png.jpg.jpg)
*Veeam Backup & Replication 13 installation initiated from mounted ISO.*


**License Agreement**

![License Agreement](https://github.com/lionelmsango/veeam-backup-disaster-recovery-lab/blob/addcbf92b0385f7b4faacb6f94affd772abad069/screenshots/3_License_Agreement.jpg_.jpg)
*Accepting Veeam end-user license agreement and third-party component licenses.*

**Installation Progress**

![Installation Progress](https://github.com/lionelmsango/veeam-backup-disaster-recovery-lab/blob/addcbf92b0385f7b4faacb6f94affd772abad069/screenshots/4_installation_progress_png.jpg.jpg)
*Installing PostgreSQL server 17.9-1 as database backend for Veeam configuration and metadata storage.*

---


