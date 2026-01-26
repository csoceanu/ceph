This documentation file serves as the manual page for `ceph-volume`, the primary CLI tool used to provision, manage, and inspect Ceph Object Storage Daemons (OSDs).

### 1. Primary Purpose
The file documents the **management of physical and logical storage devices** for use within a Ceph cluster. It outlines how to transform raw disks or LVM (Logical Volume Manager) volumes into functional Ceph OSDs, replacing the legacy `ceph-disk` utility.

### 2. Key Topics Covered
*   **Inventory Management**: How to scan and report on the host's physical disks.
*   **LVM Workflow**: The primary method for OSD deployment using LVM tags for metadata persistence. 
    *   *Sub-processes*: Batch provisioning, manual preparation (`prepare`), activation (`activate`), and combined creation (`create`).
*   **Logical Volume Operations**: Adding Write Ahead Logs (WAL), DB volumes (for BlueStore), and migrating data between volumes.
*   **Cleanup**: Procedures for "zapping" (wiping) devices to make them reusable.
*   **Legacy Support ("Simple")**: A workflow to adopt OSDs created by older tools or manual processes into the `ceph-volume` management framework.
*   **Systemd Integration**: How the tool interacts with systemd for persistence across reboots.

### 3. Technical Keywords
*   **Subcommands**: `inventory`, `lvm`, `simple`, `batch`, `prepare`, `activate`, `zap`, `migrate`, `new-wal`, `new-db`.
*   **Objectstores**: `bluestore` (default), `filestore` (implied legacy).
*   **Configuration/Flags**: `--dmcrypt` (encryption), `--crush-device-class`, `--osds-per-device`, `--block-db-size`, `--block.wal`, `--osd-fsid`.
*   **System Components**: `LVM tags`, `Logical Volumes (LV)`, `Volume Groups (VG)`, `systemd units`, `udev` (notably *not* used by this tool).
*   **Data Types**: `JSON` (for reporting and metadata persistence).

### 4. Target Audience
*   **Storage Administrators**: Who need to provision new storage nodes or replace failed drives.
*   **Site Reliability Engineers (SREs)**: Automating cluster deployments via scripts or orchestration tools (like Ansible or Salt).
*   **Developers**: Working on Ceph’s orchestration layer or storage backend.

### 5. Related Concepts
*   **ceph-osd**: The actual daemon that `ceph-volume` prepares the storage for.
*   **BlueStore**: The storage backend implementation that utilizes the `block.db` and `block.wal` devices mentioned.
*   **CRUSH Hierarchy**: The tool allows assigning OSDs to specific device classes within the CRUSH map.
*   **LVM2**: The underlying Linux technology used for volume management.
*   **ceph-disk**: The deprecated predecessor; this documentation highlights the shift away from `udev` dependency.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **CLI Argument Changes**: If new flags are added to `lvm batch` or `prepare` (e.g., new encryption formats or performance tuning parameters).
2.  **New Objectstore Support**: If a new backend (beyond BlueStore) is introduced.
3.  **Metadata Changes**: If the LVM tagging scheme or the JSON structure in `/etc/ceph/osd/` is modified.
4.  **Lifecycle Management**: Changes to how systemd units are generated or how OSDs are triggered at boot.
5.  **New Subcommands**: The addition of new top-level commands or subcommands within the `lvm` or `simple` namespaces.