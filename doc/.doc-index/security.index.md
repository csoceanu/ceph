# SECURITY Documentation Index

## Overview
This documentation serves as the central repository for Ceph's vulnerability management protocols and historical security records. It provides both procedural guidance for reporting and handling vulnerabilities and technical deep-dives into specific Common Vulnerabilities and Exposures (CVEs) to assist administrators in patching and auditing their clusters.

## Files Summary
*   **security/index.rst**: The entry point for security documentation, containing vulnerability reporting instructions, contact emails, and the policy on supported versions.
*   **security/process.rst**: Outlines the formal Vulnerability Management Process, including timelines for acknowledgement, CVE assignment, embargo policies, and disclosure workflows.
*   **security/cves.rst**: A master table of historical vulnerabilities, categorized by date, CVE ID, severity, and a brief summary.
*   **security/CVE-2021-20288.rst**: Detailed technical breakdown of the `global_id` reuse vulnerability in Cephx, including impact analysis and specific monitor configuration commands.
*   **security/CVE-2021-3509.rst**: Documentation of a Dashboard XSS vulnerability related to token cookie exposure.
*   **security/CVE-2021-3524.rst**: Details regarding HTTP header injection flaws via CORS `ExposeHeader` tags in the Rados Gateway (RGW).
*   **security/CVE-2021-3531.rst**: Documentation of a Denial of Service (DoS) vulnerability in the Swift API caused by malformed URLs.
*   **security/CVE-2022-0670.rst**: Analysis of a path-restriction bypass in the Ceph Manager "volumes" plugin affecting OpenStack Manila integrations.

## Code Changes That Would Require Documentation Updates
*   **Authentication & Authorization Logic**: Changes to Cephx ticket handling, `global_id` reclamation, or ticket Time-To-Live (TTL) logic (e.g., `mon/AuthMonitor.cc`).
*   **RGW API & Header Processing**: Modifications to how Rados Gateway handles HTTP headers, CORS configurations, or Swift API request parsing.
*   **Ceph Manager (mgr) Plugins**: Significant updates to the "volumes" plugin or any module handling external orchestration (like OpenStack Manila).
*   **Dashboard Security**: Changes to cookie handling, session management, or front-end sanitization logic that might prevent or introduce XSS.
*   **Vulnerability Response Policy**: Any change in the internal "Business Days" for acknowledgement, the 90-day embargo limit, or the list of public announcement mailing lists.
*   **Release Lifecycle**: Changes to which versions are considered "Active" or "Supported," as security patches are only backported to active releases.
*   **New Security Discoveries**: Whenever a new CVE is assigned, it must be added to the master table in `cves.rst` and a new detailed `.rst` file should be created.

## Key Technical Concepts
*   **Cephx**: The native authentication protocol; includes concepts like `global_id` and authentication tickets.
*   **CORS (Cross-Origin Resource Sharing)**: Specifically `ExposeHeader` tags and header injection mitigation.
*   **Subvolumes**: Managed by the Ceph Manager `volumes` plugin for path-restricted filesystem access.
*   **Embargo**: The period (typically 90 days) where a confirmed vulnerability is kept private to allow for patch development.
*   **CRD (Coordinated Release Date)**: The mutually agreed-upon date for public disclosure of a vulnerability.
*   **Configuration Options**: 
    *   `auth_allow_insecure_global_id_reclaim`
    *   `auth_expose_insecure_global_id_reclaim`
    *   `mon_warn_on_insecure_global_id_reclaim_allowed`
*   **Health Alerts**: `AUTH_INSECURE_GLOBAL_ID_RECLAIM` and `AUTH_INSECURE_GLOBAL_ID_RECLAIM_ALLOWED`.

## Related Components
*   **Mon (Ceph Monitor)**: Responsible for authentication (Cephx) and cluster membership.
*   **RGW (Rados Gateway)**: The object storage interface (S3/Swift compatibility).
*   **MGR (Ceph Manager)**: Handles volume management and the Ceph Dashboard.
*   **CephFS**: The distributed filesystem; specifically its path-restriction and subvolume features.
*   **OpenStack Manila**: A primary external consumer of CephFS subvolumes impacted by pathing vulnerabilities.
*   **Ceph Dashboard**: The web-based management interface.