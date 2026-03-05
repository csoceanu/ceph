This documentation file, `dev/pool-migration-design.rst`, serves as the **architectural design specification** for a non-disruptive, intra-cluster pool migration feature in Ceph (planned for the "Umbrella" release). It outlines how RADOS objects can be moved between pools (e.g., to change Erasure Coding profiles) without I/O interruption.

### 1. Primary Purpose
The file documents the high-level design, logic, and implementation strategy for migrating all objects from a source RADOS pool to a target RADOS pool within the same cluster. The goal is to allow pool transformations (Replica to EC, EC profile changes) while maintaining continuous client I/O.

### 2. Key Topics Covered
*   **Use Cases**: Changing EC profiles (K/M values), converting between replica and EC pools, and general data migration.
*   **Migration Mechanics**: Using a "watermark" hash-based approach to track progress, similar to the RADOS backfill process.
*   **I/O Routing**: How `librados` (objector) and kernel clients use the `OSDMap` and local caching to route requests to either the source or target pool during migration.
*   **Resource Scheduling**: Integration with existing OSD schedulers (mclock/WPQ) to ensure migration has a lower priority than recovery but is paced like backfill.
*   **Atomicity and Snapshots**: Handling complex objects (OMAP, attributes, and clones/snaps) to ensure space efficiency and data integrity.
*   **Stub Pools**: The concept of retaining "stub" pool information in the `OSDMap` post-migration so clients don't need to update their metadata (e.g., RBD image headers).

### 3. Technical Keywords
*   **Components/APIs**: `librados`, `objector`, `OSDMap`, `PrimaryLogPG`, `PGBackend`, `ECBackend`, `MGR`, `MON`.
*   **Commands/Operations**: `CopyFrom`, `CopyTo`, `COPY_PUT`, `PushOp`, `RWORDERED` flag.
*   **States/Flags**: `MIGRATION_WAIT`, `MIGRATING`, `watermark`, `pg_pool_t`.
*   **Configuration/Logic**: `mclock` scheduler, `pg_info_t`, hash-order listing, `EAGAIN` redirection.

### 4. Target Audience
*   **Ceph Core Developers**: Those working on RADOS, OSD peering, or client-side object routing.
*   **Quality Assurance (QA) Engineers**: To understand the state transitions and failure modes for designing Teuthology suites.
*   **Storage Architects**: To understand the performance impact and operational constraints (e.g., no PG splits during migration).

### 5. Related Concepts
*   **RADOS Backfill/Recovery**: The migration logic heavily borrows from the backfill watermark and object-pushing mechanisms.
*   **PG Splitting/Merging**: Migration interacts with PG counts; the design suggests reducing source PGs to zero as they empty.
*   **RBD Live Migration**: Distinguished from pool migration because pool migration aims for *zero* downtime, whereas RBD migration often requires a brief client-side switch.
*   **OSD Map Management**: Relates to how the Monitor (MON) manages pool metadata and distributes it to the cluster.

---

### When to Update This File
This file should be updated if code changes occur in the following areas:
1.  **State Machine Changes**: If new PG states are added or the transition logic between `MIGRATION_WAIT` and `MIGRATING` changes.
2.  **OSDMap Schema**: If the `pg_pool_t` structure is modified to include new migration metadata.
3.  **Client Routing Logic**: If the way `librados` or the kernel client handles `EAGAIN` or watermark caching is altered.
4.  **CLI/Orchestration**: If the `mgr` or `mon` commands for initiating or monitoring migration (e.g., `--migratefrom`) are renamed or their requirements change.
5.  **Feature Scope**: If support is added for "daisy-chaining" migrations, canceling migrations, or inter-cluster migration (currently explicitly excluded).