### Documentation Summary: Ceph Stretch Clusters
**File:** `rados/operations/stretch-mode.rst`

#### 1. Primary Purpose
This documentation explains how to configure and manage **Ceph Stretch Clusters**. It details a specialized operational mode designed to maintain data integrity and availability when a Ceph cluster is geographically distributed across two or three data centers. It specifically addresses how Ceph handles network splits (netsplits) and site-wide failures that would typically compromise a standard cluster's ability to remain IO-accessible.

#### 2. Key Topics Covered
*   **Architectural Strategy**: Use of a "tiebreaker" (arbiter/witness) monitor at a third site to resolve quorum in two-site deployments.
*   **Operational Modes**:
    *   **Healthy Stretch Mode**: Normal operation with replicas distributed across sites.
    *   **Degraded Stretch Mode**: Triggered when a site fails; `min_size` is reduced to 1 to maintain availability at the surviving site.
    *   **Recovery Stretch Mode**: The transition state when a site returns, ensuring data is consistent before returning to normal peering requirements.
*   **Stretch Mode vs. Stretch Pools**: Comparison between cluster-wide stretch mode and granular per-pool stretch attributes.
*   **CRUSH Integration**: Requirements for specific CRUSH rules that ensure replicas are correctly placed across buckets (e.g., `datacenter`).
*   **Monitor Election Strategy**: Implementation of the `connectivity` strategy to favor monitors with the best network health.

#### 3. Technical Keywords
*   **APIs/Commands**: 
    *   `ceph mon enable_stretch_mode <tiebreaker_mon> <crush_rule> <bucket_type>`
    *   `ceph mon set election_strategy connectivity`
    *   `ceph osd pool stretch set`
    *   `ceph mon set_location`
    *   `ceph osd force_recovery_stretch_mode`
*   **Configuration Options**: `mon_osd_min_in_ratio` (default 0.75), `min_size`, `size`.
*   **Components**: Tiebreaker monitor, Arbiter, Witness, CRUSH map, PG (Placement Group) acting sets.

#### 4. Target Audience
*   **Storage Administrators**: Managing multi-site Ceph deployments.
*   **Site Reliability Engineers (SREs)**: Designing disaster recovery (DR) and high-availability (HA) architectures.
*   **Cloud Architects**: Implementing Ceph across multiple Availability Zones (AZs).

#### 5. Related Concepts
*   **CRUSH Maps**: The underlying mechanism for data placement.
*   **Monitor Quorum**: The standard mechanism for cluster consensus.
*   **Netsplit/Partitioning**: The failure scenario stretch mode is specifically built to mitigate.
*   **Placement Groups (PGs)**: The level at which peering and replication are managed during site transitions.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:

*   **Monitor Logic**: Any changes to the `connectivity` election strategy or how monitors determine quorum during site isolation.
*   **CRUSH Engine**: If new bucket types are introduced or if the bug regarding `MAX AVAIL` reporting for multiple `take` steps is resolved (referencing Issue #56650).
*   **Feature Expansion**: If **Erasure Coding (EC)** support is added to stretch mode, or if support for **more than two data centers** or **multiple device classes** (e.g., mixing SSD/HDD in stretch mode) is implemented.
*   **CLI/Orchestration**: Changes to the syntax of `enable_stretch_mode` or the introduction of new recovery management commands.
*   **Safety Thresholds**: Changes to default ratios like `mon_osd_min_in_ratio` which impact OSD "out" marking logic.