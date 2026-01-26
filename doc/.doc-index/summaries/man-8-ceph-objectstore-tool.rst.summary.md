This documentation file describes the usage and syntax for `ceph-objectstore-tool`, a powerful offline utility used for low-level manipulation and diagnostics of Ceph Object Storage Daemons (OSDs).

### 1. Primary Purpose
The file serves as a manual page (man page) for **`ceph-objectstore-tool`**. It documents how to modify or examine the internal state of a Ceph OSD at the object level without the OSD daemon being active. It is primarily used for troubleshooting, data recovery, and manual repair of corrupted objects or Placement Groups (PGs).

### 2. Key Topics Covered
*   **Object Manipulation**: Getting/setting raw bytes, removing objects, and handling object clones.
*   **Metadata Management**: Listing and modifying Object Map (OMAP) keys, OMAP headers, and object attributes (attrs).
*   **Maintenance & Recovery**: Fixing "lost" objects, performing consistency checks (`fsck`, `repair`), and trimming PG logs.
*   **Data Portability**: Exporting and importing OSD data to files.
*   **OSD Configuration**: Interacting with the OSD superblock, OSD Maps, and creating new object stores (`mkfs`).
*   **Operational Safety**: Instructions on ensuring the OSD is stopped (`systemctl status`) before running the tool.

### 3. Technical Keywords
*   **Core Commands**: `get-bytes`, `set-bytes`, `list-omap`, `set-omap`, `get-attrs`, `rm-attrs`, `fix-lost`, `export`, `import`.
*   **Key Arguments**: `--data-path` (mandatory), `--pgid`, `--op`, `--dry-run`, `--force`, `--journal-path`.
*   **APIs/Storage Types**: `bluestore`, `memstore`, `omap`, `osdmap`, `superblock`.
*   **JSON Identifiers**: The tool uses a JSON-formatted string to uniquely identify objects (including `oid`, `key`, `snapid`, `hash`, `pool`, and `namespace`).

### 4. Target Audience
*   **Ceph Administrators**: Performing manual disaster recovery or data maintenance.
*   **Storage Engineers**: Debugging low-level data corruption or verifying object-level metadata.
*   **Developers**: Testing object-store interactions or simulating OSD failures.

### 5. Related Concepts
*   **Ceph OSD (Object Storage Daemon)**: The daemon that this tool targets.
*   **RADOS**: The underlying reliable autonomic distributed object store.
*   **BlueStore**: The primary storage implementation for Ceph OSDs referenced in the configuration options.
*   **Placement Groups (PGs)**: The logical units for distributing objects across OSDs.
*   **Systemd**: Specifically `ceph-osd@.service`, used to manage the lifecycle of the daemon before using this tool.

---

### Update Triggers for AI Systems
This documentation should be updated if code changes occur in the following areas:
1.  **New OSD Operations**: If a new subcommand is added to the `--op` argument list (e.g., a new repair or diagnostic function).
2.  **Object Metadata Structure**: Changes to how objects are identified (JSON object schema) or changes to the OMAP/Attribute storage logic.
3.  **New Storage Backends**: If Ceph adds a new storage type beyond `bluestore` or `memstore` that requires specific flags.
4.  **CLI Argument Modifications**: Adding or deprecating flags like `--data-path` or `--pgid`.
5.  **Error Handling**: Changes in how the tool handles file locks (related to the "Resource temporarily unavailable" error).