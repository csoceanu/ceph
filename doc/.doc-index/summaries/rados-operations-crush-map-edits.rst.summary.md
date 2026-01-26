This documentation file, `rados/operations/crush-map-edits.rst`, provides a technical guide for the manual modification of the Ceph **CRUSH (Controlled Replication Under Scalable Hashing) map**. It explains the lifecycle of a CRUSH map edit—from extraction to recompilation—and details the internal structural components that define data placement policy.

### 1. Primary Purpose
The file serves as the authoritative guide for **advanced administrators** who must bypass the standard Ceph CLI to perform manual adjustments to the cluster's placement hierarchy and data distribution rules. It documents the binary-to-text conversion process and the schema of the CRUSH map's internal sections.

### 2. Key Topics Covered
*   **The Editing Workflow**: The five-step process: Get → Decompile → Edit → Recompile → Set.
*   **CRUSH Map Anatomy**: Breakdown of the six main sections: `tunables`, `devices`, `types`, `buckets`, `rules`, and `choose_args`.
*   **Bucket Hierarchy**: Defining failure domains (hosts, racks, rows) and assigning weights.
*   **Bucket Algorithms**: Comparison of placement algorithms: `uniform`, `list`, `tree`, `straw`, and `straw2`.
*   **Placement Rules**: Syntax for `replicated`, `erasure`, and the newer `msr_firstn/indep` (Multi-Step Retry) rules.
*   **Migration Strategies**: Tools for transitioning legacy manual SSD/HDD hierarchies into modern "Device Classes."
*   **Tunables**: Adjusting low-level CRUSH iteration and recursion limits (e.g., `choose_total_tries`).

### 3. Technical Keywords
*   **CLI Tools**: `ceph osd getcrushmap`, `ceph osd setcrushmap`, `crushtool`.
*   **Logic Components**: `step take`, `step choose`, `step chooseleaf`, `step emit`, `step choosemsr`.
*   **Data Structures**: `bucket`, `leaf`, `root`, `device class`, `shadow tree`.
*   **Algorithms/Hashes**: `straw2`, `rjenkins1` (hash 0), `msr_indep`, `msr_firstn`.
*   **Config/Flags**: `--reclassify-root`, `--compare`, `--enable-unsafe-tunables`.

### 4. Target Audience
*   **Storage Administrators**: Specifically those managing large or heterogeneous clusters requiring custom failure domain logic.
*   **Ceph Developers**: To understand the current documentation of the CRUSH schema.
*   **System Architects**: Designing specific data durability policies across varied hardware types.

### 5. Related Concepts
*   **RADOS Gateway (RGW)**: Notable for the terminology clash where "bucket" refers to an S3-like container rather than a CRUSH hierarchy node.
*   **Peering and Placement Groups (PGs)**: The underlying mechanism that CRUSH maps influence.
*   **Device Classes**: The modern abstraction layer that largely replaces the need for manual edits.

---

### Update Triggers: When should this file be updated?
This file is sensitive to changes in the **RADOS/OSD layer**. An AI should flag this document for updates if any of the following occur in the codebase:

1.  **CRUSH Algorithm Changes**: If a new bucket algorithm (beyond `straw2`) or a new hash type is added to the `crushtool` or OSD source.
2.  **New Rule Types**: If a new replication or erasure-coding strategy is introduced (similar to how `msr_firstn` was added).
3.  **CLI Command Deprecation**: If `ceph osd getcrushmap` or `crushtool` flags are modified or replaced by new `ceph orchestrator` or `ceph dashboard` functionalities.
4.  **Schema Extensions**: If a new section is added to the CRUSH map structure (e.g., beyond the current six sections like `tunables`, `devices`, etc.).
5.  **Device Class Logic**: If the automated reclassification logic in `crushtool` changes, particularly affecting how shadow trees or root reclassification functions.