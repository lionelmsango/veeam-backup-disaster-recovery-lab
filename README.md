# Veeam Backup & Replication Lab - Implementation Journey

> *Enterprise backup infrastructure deployment using Veeam Community Edition - A learning experience in backup architecture and troubleshooting*

---

## 📋 Project Overview

**Objective:** Implement Veeam Backup & Replication infrastructure to protect Windows and Linux VMs with automated backup jobs and disaster recovery capabilities.


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


### Stage 1: Storage Preparation

**Repository Disk Partition Created**

![Repository Disk](https://github.com/lionelmsango/veeam-backup-disaster-recovery-lab/blob/addcbf92b0385f7b4faacb6f94affd772abad069/screenshots/1__repository_disk_partition_created.jpg.jpg)
*Created dedicated 199 GB partition (R:) for Veeam backup repository - separate from OS disk for better I/O performance and storage management.*

---


### Stage 2: Veeam Installation

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

### Stage 3: Console First Launch

**Veeam Console - Community Edition**

![Console First Launch](https://github.com/lionelmsango/veeam-backup-disaster-recovery-lab/blob/1e030561ddfc5f5012047c5ab89a686a85cd6afb/screenshots/5__Veeam_Console_First_Launch.jpg.jpg)
*Successfully launched Veeam Backup & Replication console. Community Edition active with 10 workload license limit (free, perpetual). Console shows main dashboard with backup job creation options.*


**Key observations:**
- Community Edition confirmed (no license key required)
- Full access to core backup features
- Console connected to localhost successfully
- Ready for infrastructure configuration

---


### Stage 4: Backup Repository Configuration

**Repository Creation Wizard**

![Add Backup Repository](screenshots/6__Add_Backup_Repository_Wizard.jpg)
*Creating backup repository "VeeamRepository01" on local storage. This repository will store all backup files (.vbk) and metadata. Description: "Local backup storage for lab VMs"*


**Repository Configuration:**
- Name: VeeamRepository01
- Type: Windows (Direct attached storage)
- Location: R:\ drive (199 GB dedicated partition)
- Purpose: Primary backup storage for all VMs


---


### Stage 5: Backup Job Configuration

**Job Summary - Windows 11 Backup**

![Backup Schedule](https://github.com/lionelmsango/veeam-backup-disaster-recovery-lab/blob/1e030561ddfc5f5012047c5ab89a686a85cd6afb/screenshots/7_Backup_Schedule_Configuration.jpg.jpg)
*Backup job "Backup-Windows11-Daily" configured successfully with the following settings:*


**Job Configuration:**
- **Name:** Backup-Windows11-Daily
- **Type:** Server-managed agent backup
- **Mode:** Entire computer (full image)
- **Protected Computer:** 192.168.10.20 (Windows 11 workstation)
- **Backup Mode:** Entire computer
- **Destination:** VeeamRepository01
- **Repository:** VeeamRepository01
- **Retention Policy:** 14 days
- **Application-aware Processing:** Enabled
- **Guest File System Indexing:** Enabled
- **Schedule:** Automatic (not set/daily)
- **GFS Retention:** Not set

This configuration enables complete system recovery with file-level restore capability.

---

---

### Stage 6: Backup Execution

**Active Backup Jobs Running**

![Backup Jobs Running](https://github.com/lionelmsango/veeam-backup-disaster-recovery-lab/blob/1e030561ddfc5f5012047c5ab89a686a85cd6afb/screenshots/8_Active_Backup_Job_Running.jpg.jpg)
*Both backup jobs started simultaneously:*


**Jobs Status:**
- **Backup-Ubuntu-Daily:** 0% completed, Linux Agent Backup, Just now, Next run: 23.03.2026 22:00
- **Backup-Windows11-Daily:** 0% completed, Windows Agent Backup, Just now, Next run: 23.03.2026 22:00

**Status:** Jobs initiated successfully, agents connected, starting data processing...

---

---

## ⚠️ Challenges Encountered

### Backup Execution Issues

**Problem:**
Backup jobs start successfully (agents connect, repository accessible) but fail during data processing phase.

**Symptoms:**
- Jobs initiate: ✓
- Agents connect: ✓
- Jobs show 0% progress
- Duration counter running
- Bottleneck shows "Detecting"
- No data processed (0 B)

**Current Troubleshooting Steps:**

1. **Verified Infrastructure:**
   - ✓ Repository accessible (199 GB free)
   - ✓ Veeam services running
   - ✓ Network connectivity (ping successful)
   - ✓ Agents deployed and responding

2. **Checking:**
   - Job logs for specific errors
   - Agent communication status
   - VSS snapshot creation
   - Firewall rules between server and VMs
   - Disk space on target VMs
   - Agent service status on targets

3. **Next Investigation:**
   - Review detailed job logs
   - Check Windows Event Viewer
   - Verify VSS writers status
   - Test manual snapshot creation
   - Check for conflicting backup software

**Key takeaway:**
This troubleshooting process demonstrates systematic problem-solving methodology. Real production environments require the ability to diagnose and resolve issues, not just configure systems that work perfectly on the first try.

---

## 📚 What I Learned

### Technical Insights

 **Backup Infrastructure Architecture**
   - Importance of dedicated backup repository storage
   - Separation of OS and backup storage for performance
   - Agent-based vs. agentless backup approaches


### Future Enhancements

**After successful backups:**
- Add more protected VMs
- Implement encryption
- Configure email notifications
- Test disaster recovery scenarios
- Set up replication jobs
- Implement SureBackup verification

---



