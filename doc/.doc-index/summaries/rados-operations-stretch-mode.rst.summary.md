This documentation provides a technical overview and operational guide for implementing **Stretch Clusters** in Ceph (RADOS). It details how to configure Ceph to maintain data integrity and availability when a cluster is geographically distributed across different data centers or availability zones.

### 1. Primary Purpose
The file documents **Stretch Mode** and **Stretch Pools**, specialized configurations designed for multi-site deployments (typically two data centers and a third tiebreaker site). Its goal is to provide strategies for handling "netsplits" (asymmetric network failures) and site-wide outages without compromising data consistency.

### 2. Key Topics Covered
*   **Operational Modes**: Distinction between "Stretch Mode" (entire cluster) and "Individual Stretch Pools" (granular per-pool control).
*   **Failure Handling**: How the cluster behaves during degraded states (site loss) and recovery states (site return).
*   **Monitor Election Strategy**: Implementation of the `connectivity` election strategy to handle site-favoritism during splits.
*   **CRUSH Topologies**: Specific requirements for CRUSH rules to ensure 2-site replication (2+2 copies).
*   **Safety Mechanisms**: Use of `min_size=1` during outages and the "Stretch Peering Rule" to prevent data loss.

### 3. Technical Keywords
*   **APIs/Commands**: 
    *   `ceph mon enable_stretch_mode` / `disable_stretch_mode`
    *   `ceph mon set_location` / `set_new_tiebreaker`
    *   `ceph osd pool stretch set`
    *   `ceph mon set election_strategy connectivity`
    *   `force_recovery_stretch_mode` / `force_healthy_stretch_mode`
*   **Configuration Options**: 
    *   `mon_osd_min_in_ratio` (default 0.75)
    *   `min_size` (dynamically adjusted between 1 and 2)
    *   `size` (typically set to 4 in stretch mode)
*   **Components**: Tiebreaker (Arbiter/Witness) Monitor, CRUSH buckets (`datacenter`), PG Acting Sets.

### 4. Target Audience
*   **Storage Administrators**: Responsible for high-availability configurations and disaster recovery.
*   **Cloud Architects**: Designing Ceph deployments across multiple availability zones.
*   **DevOps/SREs**: Managing cluster recovery and manual intervention during netsplits.

### 5. Related Concepts
*   **CRUSH Maps**: The underlying mechanism for data placement.
*   **Monitor Quorum**: The election logic required for cluster survival.
*   **Placement Groups (PGs)**: The unit of replication and peering discussed in failure scenarios.
*   **Erasure Coding (EC)**: Mentioned as currently incompatible with stretch mode.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **Monitor Logic**: Any changes to `election_strategy` or the `connectivity` score algorithm.
2.  **PG State Machine**: Modifications to how PGs transition between `degraded`, `recovery`, and `healthy` states specifically for stretch mode.
3.  **Feature Expansions**: If Ceph adds support for **Erasure Coding** or **more than two data centers** in stretch mode (currently listed as limitations).
4.  **CRUSH Engine**: Changes to how device classes (SSD/HDD) are handled, as device classes are currently unsupported in stretch rules.
5.  **CLI Changes**: Additions or deprecations of `ceph mon` or `ceph osd` commands related to "stretch" or "tiebreaker" management.