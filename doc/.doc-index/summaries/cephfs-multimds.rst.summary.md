This documentation file, `cephfs/multimds.rst`, serves as the definitive guide for scaling the Ceph File System (CephFS) using multiple active Metadata Server (MDS) daemons.

### 1. Primary Purpose
The file documents how to transition a CephFS cluster from a single active MDS to an **active-active MDS** configuration. Its goal is to explain how to scale metadata performance, manage MDS ranks, and control how metadata workload is distributed across those ranks through static and dynamic partitioning.

### 2. Key Topics Covered
*   **Scaling Logic**: When and why to use multiple active MDSs (specifically for metadata-heavy workloads with many clients).
*   **Rank Management**: Procedures for increasing and decreasing the `max_mds` setting and the lifecycle of MDS states (creating, active, stopping).
*   **High Availability**: The necessity of standby daemons to maintain quorum during rank failures.
*   **Manual Partitioning (Export Pins)**: Hard-coding specific directory trees to specific MDS ranks using extended attributes.
*   **Ephemeral Pinning**: Automatic, policy-based partitioning (Distributed and Random) based on consistent hashing of inodes.
*   **Dynamic Balancing**: Using the CephFS metadata balancer to automatically move subtrees to "colder" ranks, including granular control via rank masking.

### 3. Technical Keywords
*   **Configuration Settings**: `max_mds`, `balance_automate`, `bal_rank_mask`, `mds_export_ephemeral_random`, `mds_export_ephemeral_distributed`.
*   **Extended Attributes (xattrs)**: `ceph.dir.pin`, `ceph.dir.pin.distributed`, `ceph.dir.pin.random`.
*   **MDS States**: `up:active`, `up:creating`, `up:stopping`, `up:standby`.
*   **Commands**: `ceph fs set`, `setfattr`, `ceph status`.
*   **Concepts**: Ranks, Export Pins, Ephemeral Pinning, Consistent Hashing, Subtree Partitioning.

### 4. Target Audience
*   **Storage Administrators**: Responsible for scaling CephFS performance and ensuring High Availability.
*   **System Architects**: Designing large-scale distributed file systems.
*   **Developers/Power Users**: Needing to optimize specific application workloads by manually pinning directory metadata.

### 5. Related Concepts
*   **CephFS Administration**: General management of the file system.
*   **MDS Standby/Failover**: The mechanics of how standby daemons replace failed active ranks.
*   **Metadata Performance Tuning**: Strategies for handling high IOPS in file system metadata.
*   **Directory Fragmentation**: A prerequisite concept for how CephFS splits large directories to be managed by different MDSs.

---

### Trigger for Updates (AI/Developer Guidance)
This file should be updated if code changes occur in the following areas:
1.  **MDS State Machine**: If new MDS states are introduced or the transition logic (e.g., from `creating` to `active`) changes.
2.  **MDS Balancer Logic**: If the default behavior of the metadata balancer changes or if new balancing algorithms are added.
3.  **New Extended Attributes**: If new `ceph.dir.pin.*` attributes are implemented for metadata management.
4.  **CLI/Config Changes**: If the `max_mds` or `bal_rank_mask` commands/parameters are renamed or their syntax is altered.
5.  **Scaling Limits**: If the recommended ratios of active-to-standby daemons or ephemeral pinning thresholds are adjusted.