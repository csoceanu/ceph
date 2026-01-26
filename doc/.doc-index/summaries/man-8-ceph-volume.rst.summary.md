This documentation file serves as the manual page for `ceph-volume`, the primary utility used for deploying and managing Ceph Object Storage Daemons (OSDs).

### 1. Primary Purpose
The file documents the **`ceph-volume`** command-line tool, which is designed to provision, activate, and manage storage devices for Ceph OSDs. It focuses on using Logical Volume Manager (LVM) tags for device discovery and provides a migration path for legacy OSDs.

### 2. Key Topics Covered
*   **Inventory Management**: Scanning and reporting on physical disk metadata (model, size, SSD/HDD status).
*   **LVM Workflow**: The core lifecycle of an OSD using LVM, including:
    *   `batch`: Automated mass provisioning of OSDs.
    *   `prepare` / `activate`: The two-stage process for manual OSD setup and systemd integration.
    *   `create`: A wrapper combining preparation and activation.
    *   `zap`: Securely wiping devices or logical volumes.
*   **Advanced LVM Operations**: Adding/migrating auxiliary devices like **Write Ahead Log (WAL)** and **BlueStore Database (DB)**.
*   **Simple/Legacy Support**: Tools (`simple scan/activate`) to manage OSDs created by the deprecated `ceph-disk` utility or manual processes.
*   **Systemd Integration**: How the tool interacts with systemd units to ensure OSDs persist across reboots.

### 3. Technical Keywords
*   **Core Commands**: `inventory`, `lvm`, `simple`, `batch`, `prepare`, `activate`, `zap`, `migrate`.
*   **Configuration/APIs**: `--bluestore`, `--dmcrypt` (encryption), `--crush-device-class`, `--osds-per-device`.
*   **Storage Components**: `logical volume (lv)`, `volume group (vg)`, `block.db`, `block.wal`, `BlueFS`.
*   **Ceph Identifiers**: `osd-id`, `osd-fsid` (UUID).
*   **Integration Points**: `systemd`, `LVM tags`, `/etc/ceph/osd/` (JSON metadata).

### 4. Target Audience
*   **Storage Administrators**: Managing Ceph cluster hardware and disk provisioning.
*   **Site Reliability Engineers (SREs)**: Troubleshooting OSD boot/activation issues or disk failures.
*   **Automation Developers**: Creating scripts or Ansible playbooks to deploy Ceph (the `batch` and `json` output options are key here).

### 5. Related Concepts
*   **ceph-disk**: The legacy tool that `ceph-volume` replaces.
*   **BlueStore**: The backend object store for Ceph OSDs discussed throughout the file.
*   **LVM (Logical Volume Manager)**: The underlying Linux technology used for volume management.
*   **CRUSH Hierarchy**: Related via the `--crush-device-class` flag which affects data placement.
*   **dm-crypt**: The Linux kernel subsystem used when the `--dmcrypt` flag is invoked for encryption.

### Update Triggers for AI Systems
This file should be updated if any of the following code changes occur:
1.  **Subcommand Additions**: If a new module (beyond `lvm` or `simple`) is added to the `ceph-volume` CLI.
2.  **Flag Modifications**: If new provisioning options are added (e.g., new encryption methods, support for new object stores beyond BlueStore, or new metadata flags).
3.  **LVM Tagging Logic**: If the internal method for how Ceph identifies LVM volumes changes.
4.  **Systemd Workflow**: If the way OSDs are "triggered" or "activated" at boot time is modified.
5.  **Deprecation**: If legacy support for `simple` or `ceph-disk` formats is removed.