This documentation provides a comprehensive guide to **RBD (Ceph Block Device) Mirroring**, a feature that enables asynchronous replication of disk images between two distinct Ceph clusters.

### 1. Primary Purpose
The file documents the architecture, configuration, and management of **asynchronous replication** for RBD images. It explains how to set up disaster recovery and data redundancy by mirroring data between local and remote Ceph clusters.

### 2. Key Topics Covered
*   **Mirroring Modes**: 
    *   **Journal-based**: Uses RBD journaling for point-in-time, crash-consistent replication (higher latency, finer granularity).
    *   **Snapshot-based**: Uses deltas between periodic snapshots (lower overhead, coarser granularity).
*   **Replication Topologies**: One-way (primary to secondary) and Two-way (bidirectional) configurations.
*   **Configuration Levels**: Procedures for enabling mirroring at the **Pool**, **Namespace**, and **Image** levels.
*   **Cluster Peer Management**: Bootstrapping peers using tokens or manual configuration (keyrings/monitor hosts).
*   **Disaster Recovery Operations**: Instructions for image **promotion** (to primary), **demotion** (to non-primary), and **resyncing** after split-brain scenarios.
*   **Automation**: Scheduling periodic mirror-snapshots.
*   **The `rbd-mirror` Daemon**: Role, deployment, and permission requirements of the background worker process.

### 3. Technical Keywords
*   **Commands**: `rbd mirror pool enable`, `rbd mirror image snapshot`, `rbd mirror pool peer bootstrap create`, `rbd mirror image promote/demote`, `rbd feature enable`.
*   **Daemons**: `rbd-mirror`.
*   **Features**: `exclusive-lock`, `journaling`, `fast-diff`.
*   **Configuration Options**: `rbd_default_data_pool`, `rbd_journal_max_payload_bytes`, `rbd_mirroring_max_mirroring_snapshots`.
*   **States**: Primary, Non-primary, Split-brain, Image-mode, Pool-mode, Init-only.

### 4. Target Audience
*   **Storage Administrators**: Responsible for cluster-to-cluster data protection and disaster recovery planning.
*   **DevOps Engineers**: Automating RBD image provisioning and replication schedules.
*   **System Architects**: Designing high-availability storage solutions across multiple data centers.

### 5. Related Concepts
*   **Ceph Block Device (RBD)**: The underlying storage technology.
*   **RADOS**: The base object store (specifically pool and monitor configurations).
*   **CRUSH Maps/Failure Domains**: Relevant for multi-site deployment.
*   **Journaling**: A prerequisite feature for fine-grained replication.
*   **Namespaces**: Used for multi-tenant isolation within RBD pools.

---

### Update Triggers for AI Maintenance
This documentation should be updated if code changes occur in the following areas:
*   **CLI Changes**: Any modifications to the `rbd` command-line interface regarding the `mirror` subcommand.
*   **Mirroring Logic**: Introduction of a third mirroring mode beyond "Journal" and "Snapshot."
*   **Default Behavior**: Changes to default feature bits (e.g., if `journaling` or `exclusive-lock` are enabled by default in new releases).
*   **Daemon Requirements**: Changes in how the `rbd-mirror` daemon authenticates or connects to peers (e.g., moving away from `systemd` or changing required caps).
*   **Performance Tunables**: New configuration keys in `ceph-conf` that affect replication throughput or snapshot pruning logic.
*   **Compatibility**: When new Ceph releases change the minimum required version for specific mirroring modes (e.g., post-Octopus enhancements).