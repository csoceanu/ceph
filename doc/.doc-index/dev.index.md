# DEV Documentation Index

## Overview
This documentation area provides internal technical specifications, design patterns, and development workflows for the Ceph distributed storage system. It is intended for software engineers and contributors to understand the messenger protocol, OSD internals (including erasure coding and peering), CephFS features, and the testing infrastructure (Teuthology/vstart).

## Files Summary
*   **msgr2.rst**: Detailed specification of the msgr2 on-wire protocol, including frame formats, authentication phases, and encryption/compression handshakes.
*   **erasure-coded-pool.rst**: High-level overview of erasure-coded pool use cases, CLI interface, and profile management.
*   **perf_counters.rst**: Technical guide to Ceph's internal performance counter infrastructure, schema types, and access via the admin socket.
*   **placement-group.rst**: Conceptual notes on PG mapping algorithms, state definitions, and OMAP statistics gathering.
*   **cephfs-fscrypt.rst**: Design of CephFS file system-level encryption, covering key derivation, filename generation, and the Read-Modify-Write (RMW) write path.
*   **continuous-integration.rst**: Overview of the Ceph CI pipeline using Jenkins, Shaman, and Chacra for dependency management and binary building.
*   **pool-migration-design.rst**: Design document for non-disruptive migration of RADOS objects between pools, referencing backfill and watermark mechanisms.
*   **network-protocol.rst**: Low-level description of the Ceph network protocol messages (ACK, MSG, KEEPALIVE) and header structures.
*   **rados-client-protocol.rst**: Basics of the RADOS client-server interaction, covering request resends and OSD backoff mechanisms.
*   **osd-class-path.rst**: Troubleshooting and configuration guide for OSD rados object classes (SDK) and their loading paths.
*   **perf_histograms.rst**: Documentation on 2D performance histograms built on top of the perf counter infrastructure.
*   **release-checklists.rst**: Exhaustive checklists for the Ceph release lifecycle, including dev kickoff, freeze, and stable release steps.
*   **cephfs-snapshots.rst**: Internal design of CephFS snapshots, focusing on SnapRealms, SnapServers, and snapshot metadata storage.
*   **rbd-layering.rst**: Design of RBD copy-on-write clones, covering parent/child relationships, object overlap, and resizing logic.
*   **kubernetes.rst**: Developer guide for hacking on Ceph and Rook within a Kubernetes environment using local registries.
*   **iana.rst**: Registry of IANA assigned PEN numbers and default monitor port (3300).
*   **mon-bootstrap.rst**: Protocols and requirements for initializing new monitor clusters and expanding existing quorums.
*   **wireshark.rst**: Instructions for using and developing the Ceph protocol dissector for Wireshark.
*   **sepia.rst**: Introduction to the Sepia community test lab for active contributors.
*   **mon-osdmap-prune.rst**: Algorithm and invariants for pruning full OSDMaps in the monitor store to manage disk space.
*   **messenger.rst**: Notes on the async messenger implementation and the `ceph_perf_msgr` benchmarking tool.
*   **generatedocs.rst**: Guide for building and serving the Ceph documentation set using Sphinx and `livehtml`.
*   **delayed-delete.rst**: Logic behind lazy deletion of file data in CephFS to reduce client latency.
*   **versions.rst**: Explanation of OSD-level object versioning (eversion vs. user_version) and caching pool semantics.
*   **bluestore.rst**: Internal mapping of small write strategies (WAL, uncompressed, compressed) in the BlueStore backend.
*   **documenting.rst**: Standards for user and code documentation, including Doxygen formatting and diagram tools (Graphviz, Ditaa).
*   **blkin.rst**: Guide to request-level tracing in Ceph using LTTng, Blkin, and Zipkin.
*   **crush-msr.rst**: Specification of CRUSH Multi-step Retry (MSR) rules for improved failure domain rebalancing.
*   **health-reports.rst**: Architecture of health report aggregation across Paxos services and the Ceph Manager.
*   **development-workflow.rst**: High-level view of the Ceph release cycle, bug reporting, and PR merging processes.
*   **rbd-diff.rst**: Streaming file format specification for RBD incremental backup diffs (v1 and v2).
*   **config-key.rst**: Schema and layout for the monitor's config-key storage service (dm-crypt, mgr modules).
*   **network-encoding.rst**: Rules for serializing primitive types, structures, lists, and maps over the wire.
*   **mon-elections.rst**: Comparison of monitor election algorithms (Classic, Disallow, and Connectivity-based).
*   **mempool_accounting.rst**: Infrastructure for tracking memory consumption of C++ containers in BlueStore and other subsystems.
*   **freebsd.rst**: Implementation details for Ceph on FreeBSD, focusing on ZFS layouts and configuration paths.
*   **context.rst**: Rules for using `CephContext` and the thread-safe `dout`/`ldout` logging macros.
*   **logging.rst**: Best practices and severity levels for the Ceph cluster log.
*   **kclient.rst**: Walkthrough for testing changes to the Linux kernel CephFS driver using VMs and net namespaces.
*   **deduplication.rst**: Experimental design for RADOS-level deduplication using content hashing and chunk pools.
*   **dpdk.rst**: Configuration and compilation guide for the DPDK-based messenger stack.
*   **zoned-storage.rst**: Research and implementation status of Ceph support for Zoned Block Devices (HM-SMR/ZNS).
*   **mon-on-disk-formats.rst**: Upgrade paths and format versioning for the monitor's Auth service.
*   **cephfs-reclaim.rst**: Interface for NFS-over-CephFS state reclamation during server restarts.
*   **cpu-profiler.rst**: Usage guide for Oprofile to analyze Ceph's CPU consumption.
*   **session_authentication.rst**: Design for ongoing session message signing and authentication in the Cephx protocol.
*   **encoding.rst**: Fundamental rules and macros (`ENCODE_START`) for ensuring forward/backward compatibility of data serialization.
*   **quick_guide.rst**: Rapid onboarding for building Ceph and running a development cluster via `vstart.sh`.
*   **release-process.rst**: Step-by-step instructions for building, signing, and publishing official Ceph releases and containers.
*   **cephx_protocol.rst**: Deep dive into the Cephx authentication phases (Phase I and II) based on Kerberos.
*   **vstart-ganesha.rst**: Guide for deploying NFS Ganesha with CephFS or RGW exports in a development environment.
*   **internals.rst**: Entry point for core Ceph internal documentation and community resources.
*   **dev_cluster_deployment.rst**: Comprehensive list of `vstart.sh` options and environment variables for local testing.
*   **balancer-design.rst**: Rationale and mechanism for capacity (upmap) and read balancing in the cluster.
*   **rbd-export.rst**: Specification of the RBD image export/import file format.
*   **libcephfs_proxy.rst**: Design of the `libcephfs_proxy` and `libcephfsd` for centralizing metadata and data caching.
*   **libs.rst**: High-level overview of Ceph library architecture (`libcommon`, `libglobal`).
*   **peering.rst**: Comprehensive reference for the PG peering process, intervals, and authoritative history reconstruction.
*   **file-striping.rst**: Mathematical rules for how CephFS files are striped into RADOS objects.
*   **logs.rst**: Developer guide for debug logs (`dout`) and performance counters API.
*   **perf.rst**: Instructions for using the Linux `perf` tool and generating Flamegraphs for OSD analysis.
*   **testing.rst**: Notes on building integration branches and pushing to `ceph-ci`.
*   **cputrace.rst**: Technical guide for `CpuTrace`, a hardware-counter-based tool for measuring instruction costs.
*   **cxx.rst**: Status of C++17 adoption and implications of the `libstdc++` ABI on various distros.
*   **object-store.rst**: Architectural diagram of the OSD object store layers (Objecter, BlueStore, RocksDB).
*   **ceph_krb_auth.rst**: Setup guide for Kerberos authentication using GSSAPI and SASL.
*   **corpus.rst**: Process for generating and maintaining the encoded object corpus for upgrade testing.
*   **config.rst**: Internal mechanics of the configuration management system, metavariables, and observers.
*   **seastore.rst**: Future design for a Seastar-based OSD backend (SeaStore) optimized for NVMe and user-space I/O.
*   **cephx.rst**: Protocol overview of Cephx tickets, challenges, and rotating service secrets.
*   **macos.rst**: Guide for building Ceph components (primarily FUSE) on macOS.
*   **cephfs-mirroring.rst**: Design and interface for asynchronous snapshot mirroring in CephFS.

## Code Changes That Would Require Documentation Updates
*   **Messenger modifications**: Any changes to frame structures, encryption logic in `AsyncMessenger`, or adding new messenger tags (e.g., `CEPH_MSGR_TAG_*`).
*   **Paxos/Monitor service updates**: Adding new monitor features, changing Auth format versions, or altering the OSDMap pruning/trimming logic.
*   **New configuration options**: Adding options to `.yaml.in` files requires updates to `config.rst` and related subsystem docs.
*   **Serialization changes**: Modifying `encode`/`decode` logic in core structures (`object_info_t`, `pg_info_t`) or changing the `ENCODE_START` macro versioning rules.
*   **OSD Backend enhancements**: Changes to BlueStore write strategies, memory pool allocations, or SeaStore's segment management.
*   **Peering logic adjustments**: Altering the PG state machine, interval calculation, or authoritative history selection.
*   **CephFS feature additions**: Modifying snapshot metadata, the `fscrypt` write path, or mirroring protocols.
*   **RBD format updates**: Changes to image export/import formats, diff versioning (v3+), or CO-WRITE layering logic.
*   **CI/Release automation**: Modifying Jenkins job parameters, signing scripts, or the release kickoff checklist.
*   **Testing Tooling**: Updates to `vstart.sh`, `teuthology-suite`, or the introduction of new perf benchmarking flags.

## Key Technical Concepts
*   **Peering**: Acting Set, Up Set, PG Temp, Authoritative History, Interval.
*   **Networking**: Frame, Preamble, Epilogue, msgr2, GCM, CRC32-C.
*   **Storage Backends**: BlueStore, WAL, RocksDB, SeaStore, ZNS, SMR.
*   **Auth**: Cephx, Ticket, Authorizer, Principal, GSSAPI, SASL.
*   **CephFS**: SnapRealm, Inode Quiescelock, Subtree Migration, CapSnap.
*   **RBD**: Layering, Overlap, Flatten, rbd-diff.
*   **Management**: Paxos Service, Config-Key, Health Reports, Upmap, Balancer.
*   **Data Structures**: hobject_t, ghobject_t, eversion_t, bufferlist.

## Related Components
*   **OSD**: PrimaryLogPG, PGBackend, BlueStore, ObjectStore.
*   **Monitor**: AuthMonitor, OSDMonitor, PaxosService, ElectionLogic.
*   **Manager**: MgrMonitor, MgrStatAggregator, Dashboard modules.
*   **Client**: Librados, Objecter, kRBD, libcephfs.
*   **Infrastructure**: Teuthology, Jenkins, Shaman, Chacra, Sepia Lab.