This documentation provides a technical overview and operational guide for **libcephsqlite**, a SQLite Virtual File System (VFS) that enables SQLite databases to be stored directly on **RADOS** (Ceph’s object store).

### 1. Primary Purpose
The file documents the integration between SQLite and Ceph, allowing developers to use Ceph as a backing storage layer for SQLite. It specifically positions this tool as a way to decentralize a database across Ceph's object store for improved availability, while explicitly clarifying that it is **not** a distributed SQL engine—it is designed for single-client access at any given time.

### 2. Key Topics Covered
*   **Architecture & Concurrency**: Explains the "exclusive access" model using RADOS locks and the necessity of blocklisting dead clients to prevent corruption.
*   **Usage & URI Formatting**: Details how to load the VFS extension and the specific URI syntax (`file:///<pool>:[namespace]/<dbname>?vfs=ceph`) required to connect.
*   **Performance Optimization**: Recommendations for `page_size`, `cache_size`, `journal_mode` (PERSIST), and `locking_mode` (EXCLUSIVE) to mitigate latency overhead inherent in network storage.
*   **Database Management**: Methods for extracting databases from RADOS using the `rados --striper` command and the use of the SQLite Backup API.
*   **Safety & Recovery**: Procedures for breaking stale locks, handling client crashes, and the role of the `ceph-mgr` in automated blocklisting.
*   **Observability**: Instructions on using custom SQL functions (`ceph_status()`, `ceph_perf()`) and debug flags to monitor VFS health and performance.

### 3. Technical Keywords
*   **APIs**: SQLite VFS API, SQLite Extension Loading API, RADOS Striper API.
*   **Configuration Options**: `cephsqlite_lock_renewal_timeout`, `cephsqlite_lock_renewal_interval`, `cephsqlite_blocklist_dead_locker`.
*   **SQL Pragmas**: `PRAGMA page_size`, `PRAGMA journal_mode = PERSIST`, `PRAGMA locking_mode = EXCLUSIVE`, `PRAGMA temp_store`.
*   **Commands**: `.load libcephsqlite.so`, `ceph osd blocklist add`, `rados --striper get`.
*   **Environment Variables**: `CEPH_CONF`, `CEPH_KEYRING`, `CEPH_ARGS`.

### 4. Target Audience
*   **Ceph Developers**: Specifically those working on `ceph-mgr` or other internal Ceph components that require relational storage.
*   **Database Administrators/SREs**: Responsible for managing Ceph cluster permissions and troubleshooting locked or corrupted databases.
*   **Software Engineers**: Looking to store application state in a consistent, multi-object-spanning format on RADOS without implementing a custom striping engine.

### 5. Related Concepts
*   **RADOS Striper**: The underlying mechanism that breaks the SQLite file into multiple RADOS objects.
*   **RBD (RADOS Block Device)**: Used as a conceptual comparison for exclusive-access storage logic.
*   **Ceph Manager (ceph-mgr)**: A primary consumer of this VFS for internal state management.
*   **Blocklisting**: The mechanism for fencing clients to maintain data integrity.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:
*   **URI Parsing**: If the logic for identifying pools, namespaces, or the `vfs=ceph` parameter changes.
*   **Locking Logic**: If the default timeouts, renewal intervals, or the specific RADOS lock names (`striper.lock`) are modified.
*   **Performance Counters**: If new performance metrics are added to the `ceph_perf()` JSON output or if the VFS API implementation changes from asynchronous to synchronous.
*   **Auth Profiles**: If the `simple-rados-client-with-blocklist` profile or required capabilities (e.g., specific monitor commands) are updated.
*   **Dependency Changes**: If the minimum supported SQLite version changes or if the `JSON1` extension becomes a mandatory requirement for core functionality.