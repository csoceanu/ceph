This documentation provides a technical overview of the **Ceph Manager (mgr) Balancer Module**, which automates and optimizes the distribution of Placement Groups (PGs) across Object Storage Daemons (OSDs).

### 1. Primary Purpose
The file documents the configuration and operation of the Ceph balancer. Its primary goal is to ensure even data distribution across the cluster to prevent individual OSDs from becoming hotspots or reaching capacity prematurely, while also optimizing for read performance in newer versions.

### 2. Key Topics Covered
*   **Operational Modes**: Detailed explanation of the four balancing strategies (`upmap`, `crush-compat`, `read`, and `upmap-read`).
*   **Automation & Scheduling**: How to enable/disable the balancer and restrict its activity to specific time windows, weekdays, or pools.
*   **Throttling & Safety**: Parameters to control the speed of data remapping to prevent performance degradation during balancing.
*   **Supervised Optimization**: A manual workflow (eval -> optimize -> execute) for administrators who want to preview and approve PG movements.
*   **Status Monitoring**: Commands to inspect the current state of balancing and the quality of distribution.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph balancer status`, `ceph balancer mode <mode>`, `ceph balancer optimize <plan>`, `ceph balancer execute`, `ceph osd df`.
*   **Configuration Options**: 
    *   `target_max_misplaced_ratio`: Threshold for maximum data movement.
    *   `upmap_max_deviation`: The allowed variance of PGs per OSD.
    *   `sleep_interval`, `begin_time`, `end_time`, `begin_weekday`, `end_weekday`: Scheduling variables.
    *   `mgr/balancer/pool_ids`: Scoping the balancer to specific pools.
*   **Internal Mechanisms**: `upmap` entries, `pg-upmap-primary`, `compat weight-set`, CRUSH hierarchy.

### 4. Target Audience
*   **Storage Administrators**: Seeking to optimize cluster performance and storage efficiency.
*   **System Engineers**: Responsible for cluster maintenance and ensuring high availability during reweighting.
*   **DevOps/SREs**: Implementing automation for Ceph cluster lifecycle management.

### 5. Related Concepts
*   **CRUSH Maps**: The underlying algorithm the balancer attempts to optimize or bypass via upmaps.
*   **PG (Placement Group) Management**: The fundamental unit of data distribution in Ceph.
*   **MGR Modules**: The infrastructure within Ceph that hosts the balancer logic.
*   **Client Compatibility**: Relation to Ceph releases (Luminous, Reef, etc.) and how different modes require specific client-side features.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
*   **New Balancing Modes**: If a new mode (beyond the current four) is added to the `mgr/balancer` module.
*   **Default Value Changes**: If the default `upmap_max_deviation` (currently 5) or `target_max_misplaced_ratio` (currently 5%) changes in the source code.
*   **New Configuration Keys**: If new parameters are added to the `MGR` config options for the balancer (e.g., new throttling metrics).
*   **Command Syntax**: If the CLI interface for `ceph balancer` is modified or deprecated.
*   **Minimum Client Requirements**: If enhancements to the `upmap` or `read` logic change the required minimum Ceph version for clients.