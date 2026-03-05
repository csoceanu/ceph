This analysis summarizes the `cephfs/disaster-recovery-experts.rst` documentation to assist in maintenance and contextual understanding.

### 1. Primary Purpose
The file serves as a high-level technical guide for **manual metadata repair and disaster recovery** of a Ceph File System (CephFS). it provides procedures for situations where the Metadata Server (MDS) cannot join the cluster or replay its journal due to corruption or missing RADOS objects.

### 2. Key Topics Covered
*   **Journal Management**: Exporting, importing, and truncating (resetting) the MDS journal.
*   **Metadata Extraction**: Recovering dentries and inodes from a damaged journal into the backing store.
*   **Table Repairs**: Resetting the Session Table, Inode Table (InoTable), and SnapServer when RADOS objects are missing or inconsistent.
*   **MDS Map Manipulation**: Forcing a filesystem reset to a single MDS rank.
*   **Disaster Recovery from Data Pool**: Using `cephfs-data-scan` to reconstruct metadata by scanning the data pool (extents, inodes, and links).
*   **Parallelization**: Instructions for using multiple workers to speed up the time-consuming data scanning process.
*   **Advanced Recovery Strategy**: Creating a "Recovery Filesystem" using an alternate metadata pool to safely reconstruct metadata without modifying the original damaged pool.

### 3. Technical Keywords
*   **Tools**: `cephfs-journal-tool`, `cephfs-table-tool`, `cephfs-data-scan`, `rados`, `ceph-dencoder`.
*   **Commands**: `journal export/import`, `event recover_dentries`, `journal reset`, `reset session/snap/inode`, `fs reset`, `scan_extents`, `scan_inodes`, `scan_links`.
*   **Configuration/Flags**: `--rank`, `--worker_n`, `--worker_m`, `--alternate-pool`, `--recover`, `--allow-dangerous-metadata-overlay`.
*   **State Terms**: `down:damaged`, `up:replay`, `in` state, `backing store`.

### 4. Target Audience
*   **Ceph Administrators** dealing with critical filesystem failures.
*   **Storage Engineers** with expert-level knowledge of CephFS internals.
*   **Support Personnel** performing manual recovery after PG loss or metadata corruption.

### 5. Related Concepts
*   **MDS Internals**: Understanding how the MDS uses journals and tables (Session, Snap, Ino) to track filesystem state.
*   **CephFS Scrub**: The necessary follow-up action to ensure consistency after manual repairs.
*   **RADOS**: The underlying object store where CephFS metadata and data pools reside.
*   **Quincy Release**: Specifically mentioned regarding improvements in symbolic link recovery.

---

### Update Triggers for AI Systems
This file should be reviewed or updated if changes occur in the following areas of the Ceph codebase:

1.  **Tooling Syntax**: Changes to the CLI arguments or subcommands in `cephfs-journal-tool`, `cephfs-table-tool`, or `cephfs-data-scan`.
2.  **Metadata Format**: Changes to the serialization/encoding of inodes, dentries, or MDS tables (e.g., updates in `ceph-dencoder` related to FS).
3.  **Automation**: The documentation mentions a future "Disaster Recovery Super Tool." If this is released, large sections of this manual guide may become deprecated.
4.  **MDS State Machine**: Changes in how the MDS handles `down:damaged` or journal replay logic.
5.  **Recovery Features**: New capabilities in the `scrub` mechanism or new recovery flags (like the Quincy symlink fix) should be reflected here.