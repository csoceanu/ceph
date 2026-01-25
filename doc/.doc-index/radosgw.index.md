# RADOSGW Documentation Index

## Overview
This documentation covers the Ceph Object Gateway (RGW), a RESTful interface built on top of `librados` that provides applications with Amazon S3 and OpenStack Swift-compatible object storage. It details administration, multi-site replication, security integrations (encryption/IAM/Key Management), performance tuning, and advanced features like Lua scripting and cloud transitions.

## Files Summary
*   **s3_objects_dedup.rst:** Documentation for the `radosgw-admin dedup` commands used to estimate and execute object tail deduplication to save storage.
*   **vault.rst:** Detailed configuration guide for integrating HashiCorp Vault for Server-Side Encryption (SSE) using KV or Transit engines.
*   **mfa.rst:** Covers Multi-Factor Authentication (MFA) support for bucket object deletion via the S3 API.
*   **opa.rst:** Explains integration with Open Policy Agent (OPA) for offloading authorization decisions.
*   **archive-sync-module.rst:** Describes the Archive Sync module which maintains an immutable history of object versions in a dedicated zone.
*   **config-ref.rst:** Comprehensive reference for RGW configuration parameters, including GC, lifecycle, multisite, and QoS settings.
*   **iam.rst:** Lists supported Amazon IAM API subset actions for managing users, roles, and policies within RGW.
*   **session-tags.rst:** Detailed guide on using Attribute-Based Access Control (ABAC) via STS session tags.
*   **pools.rst:** Describes the underlying RADOS pool structure used by RGW zones and namespaces.
*   **kmip.rst:** Instructions for using KMIP-compatible key managers (like IBM SKLM) for object encryption.
*   **frontends.rst:** Technical details for configuring HTTP frontends, specifically the `beast` and `civetweb` libraries.
*   **role.rst:** Admin guide and REST API reference for creating and managing IAM-style roles.
*   **s3.rst:** Feature support matrix and SDK overview for the S3-compatible interface.
*   **multisite-sync-policy.rst:** Documentation for fine-grained, bucket-level data movement policies between zones.
*   **troubleshooting.rst:** Common error scenarios, debugging techniques via admin sockets, and metadata heap cleanup.
*   **admin.rst:** Core administration guide covering user management, subusers, quotas, and capabilities.
*   **bucket_logging.rst:** Explains the mechanism for logging all access requests to specific buckets into a separate log bucket.
*   **cloud-transition.rst:** Details unidirectional lifecycle-based transition of objects to external cloud providers (AWS S3).
*   **sync-modules.rst:** High-level overview of the multisite sync module framework (ElasticSearch, Cloud, Archive).
*   **oidc.rst:** REST API reference for managing OpenID Connect Provider entities.
*   **multitenancy.rst:** Explains the isolation of users and buckets using the `tenant$user` naming convention.
*   **nfs.rst:** Guide for exporting RGW namespaces via NFS-Ganesha using `librgw`.
*   **zone-features.rst:** Lists RGW features that require cluster-wide support (resharding, notification_v2).
*   **notifications.rst:** Deep dive into bucket notifications via HTTP, AMQP, and Kafka endpoints.
*   **swift.rst:** Feature support matrix and basic operations for the OpenStack Swift-compatible interface.
*   **account.rst:** Details the "User Accounts" feature for self-service IAM management and resource ownership.
*   **dynamicresharding.rst:** Explains automatic and manual bucket index resharding to maintain performance.
*   **keycloak.rst:** Configuration guide for using Keycloak as an OIDC provider for S3/STS authentication.
*   **api.rst:** Minimal reference for the `rgw` Python module bindings.
*   **lua-scripting.rst:** Technical guide for executing Lua scripts in request/data contexts (Pacific release+).
*   **cloud-restore.rst:** Documentation for restoring objects that were previously transitioned to remote cloud storage.
*   **encryption.rst:** General overview of Server-Side Encryption (SSE-C, SSE-KMS, SSE-S3).
*   **keystone.rst:** Guide for integrating OpenStack Keystone for identity management and service tokens.
*   **s3select.rst:** Details the S3 Select engine for querying object data using SQL-like syntax.
*   **index.rst:** The main entry point and toctree for the RGW documentation.
*   **ldap-auth.rst:** Instructions for delegating RGW authentication to an LDAP or Active Directory server.
*   **s3-notification-compatibility.rst:** Lists differences between RGW and AWS bucket notification implementations.
*   **elastic-sync-module.rst:** Guide for syncing object metadata to an ElasticSearch cluster for indexing.
*   **qat-accel.rst:** Documentation for Intel QAT hardware acceleration for encryption and compression.
*   **adminops.rst:** Reference for the RGW Admin Ops REST API (usage, user info, etc.).
*   **placement.rst:** Details on Placement Targets, Storage Classes, and mapping them to RADOS pools.
*   **cloud-sync-module.rst:** Covers syncing an entire zone's data to a remote S3 cloud endpoint.
*   **barbican.rst:** Guide for using OpenStack Barbican as a Key Management Service.
*   **metrics.rst:** Overview of performance counters and Prometheus integration for RGW monitoring.
*   **layout.rst:** Explains the internal data layout of RGW metadata and objects within RADOS.
*   **compression.rst:** Guide for configuring RGW-level compression (lz4, zlib, etc.).
*   **orphans.rst:** Details tooling for finding and removing leaked RADOS objects.
*   **d3n_datacache.rst:** Details the D3N L1 local data cache for speeding up reads via SSD/NVMe.
*   **uadk-accel.rst:** Guide for using HiSilicon UADK acceleration for compression algorithms.
*   **STS.rst:** Comprehensive guide for the Secure Token Service (AssumeRole, WebIdentity).
*   **STSLite.rst:** Details the extension for caching external identity provider tokens.
*   **bucketpolicy.rst:** Reference for supported S3 Bucket Policy actions and condition keys.

## Code Changes That Would Require Documentation Updates
*   **Configuration Parameters:** Adding, removing, or changing default values for any `rgw_*` option in `src/common/options/rgw.yaml.in`.
*   **Admin CLI Changes:** Modifying `radosgw-admin` subcommands, arguments, or output formats (e.g., adding a new `reshard` or `user` flag).
*   **API Compatibility:** Updating the support status of S3 or Swift functional features (e.g., adding support for a new S3 Header or Action).
*   **Encryption Backends:** Changes to how RGW communicates with Vault, Barbican, or KMIP providers, or adding a new KMS plugin.
*   **Internal Data Layout:** Modifying how objects are striped, how metadata is stored in omap, or changes to bucket index sharding logic.
*   **Multisite Sync:** Adding new sync modules, changing the sync policy engine logic, or modifying the replication log format.
*   **Performance Acceleration:** Updates to Intel QAT, UADK, or compression plugin integrations.
*   **Auth & IAM:** Modifying policy evaluation logic, adding new condition keys for bucket policies, or changing OIDC/STS credential handling.
*   **Caching Layers:** Changes to D3N, Nginx-based caching, or the internal metadata cache (LRU).
*   **Lua Scripting:** Adding new fields to the `Request` table available to Lua scripts or changing existing field behaviors.
*   **Bucket Operations:** Changes to bucket lifecycle (LC) processing, garbage collection (GC) intervals, or bucket logging formats.

## Key Technical Concepts
*   **Commands:** `radosgw-admin`, `rgw-orphan-list`, `ceph config`, `aws s3api`, `vault`, `openstack ec2`.
*   **Core Entities:** Realm, Zonegroup, Zone, Placement Target, Storage Class, Tenant, User, Subuser, Role, Account.
*   **APIs:** S3 (compatible), Swift (compatible), Admin Ops, IAM, STS, S3 Select.
*   **Security:** SSE-C, SSE-KMS, SSE-S3, OIDC, SAML, LDAP, Keystone, Barbican, OPA, MFA, HMAC-SHA1.
*   **Mechanisms:** Resharding (Dynamic/Manual), Lifecycle (LC), Garbage Collection (GC), Multipart Upload, Sharding, Sync Modules.
*   **Internals:** Omap, XATTR, Head/Tail Objects, Manifest, Bucket Index, RADOS Namespaces.
*   **Networking:** HTTP Frontends (Beast, Civetweb), Virtual-hosted vs Path-style access.

## Related Components
*   **RADOS:** The underlying object store (OSDs and Monitors).
*   **NFS-Ganesha:** Used for RGW NFS exports.
*   **Identity Providers:** Keycloak, Active Directory, OpenStack Keystone.
*   **External Cloud:** AWS S3 (for transition/sync).
*   **Monitoring:** Prometheus, `ceph-exporter`.
*   **Hardware:** Intel QAT, HiSilicon Kunpeng (UADK).
*   **External Engines:** ElasticSearch (for metadata indexing), Open Policy Agent.