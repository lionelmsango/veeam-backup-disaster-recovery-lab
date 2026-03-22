# Veeam Backup & Replication Lab - Building Enterprise Backup Infrastructure

---

## What I Built (So Far)

This lab demonstrates the foundation of a complete backup infrastructure:

- ✅ Windows Server 2022 backup server deployment
- ✅ Veeam Backup & Replication 13 installation and configuration  
- ✅ 200GB dedicated backup repository setup
- ✅ Two automated backup jobs created (Windows 11 and Ubuntu VMs)
- ⚠️ Backup execution troubleshooting (in progress)

**Current Status:** Infrastructure operational, backup jobs configured, troubleshooting initial backup failures.

**Current Status:** Infrastructure operational, backup jobs configured, troubleshooting initial backup failures.

---

## The Architecture

```
┌──────────────────────────────────────────────────────────┐
│              Lab Network                  │
├──────────────────────────────────────────────────────────┤
│                                                           │
│  ┌──────────────────────────────────────┐                │
│  │   Veeam Backup Server                │                │
│  │   Windows Server 2022                │                │
│  │   IP: 192.168.10.10                  │                │
│  │                                      │                │
│  │   Specs:                             │                │
│  │   • 8 GB RAM                         │                │
│  │   • 4 CPU cores                      │                │
│  │   • C:\ 100 GB (System)              │                │
│  │   • R:\ 200 GB (Backup Repository)   │                │
│  │                                      │                │
│  │   Services:                          │                │
│  │   • Veeam Backup Service            │                │
│  │   • PostgreSQL Database             │                │
│  │   • Backup Repository               │                │
│  └──────────────┬───────────────────────┘                │
│                 │                                         │
│                 │ Agent-Based Backups                     │
│                 │                                         │
│    ┌────────────┴────────────────────┐                   │
│    │                                 │                   │
│    ▼                                 ▼                   │
│  ┌────────────┐                ┌────────────┐           │
│  │ Windows 11 │                │   Ubuntu   │           │
│  │     VM     │                │     VM     │           │
│  │            │                │            │           │
│  │ .10.20     │                │ .10.30   │           │
│  └────────────┘                └────────────┘           │
│                                                           │
│  Protected VMs: 2                                        │
│  Backup Jobs: 2                                          │
│                                                           │
└──────────────────────────────────────────────────────────┘

```

## Phase 1: Building the Foundation

### The Backup Server

Every backup infrastructure starts with a dedicated backup server. I deployed **Windows Server 2022** as the foundation.
- **Static IP (192.168.10.10):** Backup servers need consistent addresses
- **Dual disks:** Separate system (C:) and repository (R:) drives
- **200GB repository:** Sized for 14 days of incremental backups
- **Domain-joined:** Not required for this lab, but production-ready architecture

I chose to use a **dedicated repository drive** (R:\) rather than storing backups on the system drive. In production, backup repositories should never share disk I/O with the operating system - this prevents backup jobs from impacting server performance and vice versa.

(project to be continued)
