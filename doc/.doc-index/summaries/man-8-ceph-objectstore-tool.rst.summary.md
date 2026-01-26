This documentation file serves as the manual page for `ceph-objectstore-tool`, a critical low-level diagnostic and recovery utility for Ceph storage clusters.

### 1. Primary Purpose
The file documents the usage, syntax, and capabilities of the `ceph-objectstore-tool`. This utility is designed for **offline** manipulation and examination of a Ceph Object Storage Device (OSD). It allows administrators to bypass the standard Ceph protocol to fix corruption, recover lost objects, or export/import data directly from the OSD's underlying storage.

### 2. Key Topics Covered
*   **Operational Requirements**: Emphasis on the requirement that the OSD daemon must be stopped (`systemctl stop ceph-osd@$ID`) before use to avoid data corruption or "Resource temporarily unavailable" errors.
*   **Object Manipulation**: Procedures for getting/setting raw bytes, removing objects, and handling object clones.
*   **Metadata Management**: Detailed instructions for listing and modifying Object Map (OMAP) keys, OMAP headers, and object attributes (attrs).
*   **Troubleshooting & Recovery**: Commands for fixing "lost" objects, performing `fsck` on the store, repairing PGs, and trimming PG logs.
*   **Data Portability**: Exporting and importing OSD data or specific Placement Groups (PGs).
*   **System Integration**: Interaction with OSD maps, superblocks, and monitor databases.

### 3. Technical Keywords
*   **Core Commands (`--op`)**: `info`, `log`, `remove`, `mkfs`, `fsck`, `repair`, `fuse`, `export`, `import`, `fix-lost`, `trim-pg-log`, `list-pgs`.
*   **Object Arguments**: `get-bytes`, `set-bytes`, `list-omap`, `get-omaphdr`, `set-attrs`, `rm-omap`.
*   **Configuration/Flags**: `--data-path`, `--pgid`, `--osd-type` (bluestore/memstore), `--journal-path`, `--dry-run`, `--force`, `--format`.
*   **Data Structures**: OMAP (Object Map), PG (Placement Group), Superblock, OSDMap, Epoch, fsid.

### 4. Target Audience
*   **Storage Administrators**: For emergency data recovery and manual cluster repair.
*   **Site Reliability Engineers (SREs)**: For troubleshooting deep-seated OSD inconsistencies.
*   **Ceph Developers**: For testing object-store behaviors or manually injecting states for debugging.

### 5. Related Concepts
*   **Ceph OSD (Object Storage Device)**: The underlying service being manipulated.
*   **BlueStore/FileStore**: The storage backend engines that `ceph-objectstore-tool` interacts with.
*   **Placement Groups (PGs)**: The logical collection of objects that this tool often filters by.
*   **RADOS**: The fundamental object store layer of Ceph.

---

### Maintenance Triggers: When to update this file
An AI system should flag this documentation for updates if code changes occur in the following areas:

1.  **Command-Line Interface (CLI) Changes**: If new arguments are added to `ObjectStoreTool.cc` or if the syntax for positional arguments changes.
2.  **New Backend Support**: If a new storage engine is introduced (beyond BlueStore/MemStore) that requires a new `--type` argument.
3.  **New Recovery Operations**: If new recovery logic is added to the OSD code (e.g., a new way to handle "scrub" errors or a new metadata repair tool).
4.  **JSON Schema Changes**: If the JSON structure used to identify objects (containing `oid`, `key`, `snapid`, `hash`, etc.) is modified, as the examples in the doc rely heavily on this format.
5.  **New PG/Object Attributes**: If fundamental metadata types are added to the Ceph object store that require new `get/set` operations.