This documentation file, `rbd-live-migration.rst`, serves as the definitive guide for moving Ceph Block Device (RBD) images between different locations or formats while maintaining availability.

### 1. Primary Purpose
The file documents the **RBD Live Migration** feature. It explains how to move RBD images between pools, clusters, or from external sources to a Ceph cluster with minimal downtime. It details the workflow for deep-copying image data and snapshot history while allowing clients to continue I/O operations on the new target.

### 2. Key Topics Covered
*   **Migration Modes**: 
    *   *Standard Migration*: Between pools/layouts within the same cluster (source becomes read-only).
    *   *Import-Only Migration*: From external sources or other Ceph clusters (source remains unmodified).
*   **Three-Step Operational Workflow**:
    1.  **Prepare**: Creating the target and linking it to the source.
    2.  **Execute**: The background deep-copy process.
    3.  **Finish (Commit/Abort)**: Finalizing the transition or reverting changes.
*   **Supported External Sources**: Detailed JSON specifications for migrating data from `file`, `http(s)`, `S3`, and `NBD` streams.
*   **Supported Formats**: Handling of `native` (RBD), `qcow`/`qcow2`, and `raw` image formats.

### 3. Technical Keywords
*   **CLI Commands**: `rbd migration prepare`, `rbd migration execute`, `rbd migration commit`, `rbd migration abort`, `rbd status`, `rbd trash`.
*   **Configuration Options**: `--import-only`, `--source-spec`, `--source-spec-path`, `--force`.
*   **Source-Spec JSON components**: `type`, `stream`, `pool_name`, `image_id`, `snap_name`, `url`, `access_key`.
*   **States**: `prepared`, `executing`, `executed`.
*   **Version Requirements**: Ceph Nautilus (Basic), Ceph Pacific (External sources).

### 4. Target Audience
*   **Storage Administrators**: Performing cluster maintenance, pool rebalancing, or hardware refreshes.
*   **Cloud Operators**: Migrating VM images from external providers (S3/HTTP) into a Ceph-backed environment.
*   **DevOps Engineers**: Automating image deployment or format conversions (e.g., converting QCOW2 to RBD).

### 5. Related Concepts
*   **RBD Layering/Clones**: The migration mechanism mimics "layered images" by redirecting reads to the source for uninitialized blocks.
*   **Sparse Allocation**: The process attempts to preserve sparseness during the deep-copy.
*   **RBD Trash**: Used to temporarily store source images during migration to prevent accidental usage.
*   **NBD (Network Block Device)**: One of the supported streaming protocols for data import.

---

### Update Triggers for AI Maintenance
An AI system should flag this file for updates if code changes occur in the following areas:
1.  **New Stream Types**: If support is added for new protocols (e.g., SFTP, Azure Blob).
2.  **New Format Support**: If `qcow2` advanced features (encryption, compression) or new `rbd export-format v2` features are implemented.
3.  **CLI Changes**: Any modification to the `rbd migration` subcommand syntax or the addition of new flags.
4.  **Kernel Support**: If the `krbd` module gains support for live migration (currently listed as unsupported).
5.  **State Machine Alterations**: If the internal steps of migration (Prepare/Execute/Commit) change or if new states are added to `rbd status`.