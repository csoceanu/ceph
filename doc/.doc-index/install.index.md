# INSTALL Documentation Index

## Overview
This documentation area provides comprehensive instructions for acquiring, building, and installing Ceph across diverse environments including Linux (APT/RPM), Windows, and FreeBSD. It covers the full deployment lifecycle from initial repository configuration and manual bootstrapping of core daemons (Mon, OSD, MDS, RGW) to virtualization integration and containerized delivery.

## Files Summary
*   **install/install-vm-cloud.rst:** Detail instructions for installing QEMU/KVM and libvirt to enable Ceph Block Devices as backends for VMs and cloud platforms.
*   **install/containers.rst:** Guide to official and legacy Ceph container images, explaining naming conventions, registry locations (Quay/Docker Hub), and tag meanings.
*   **install/windows-install.rst:** Prerequisites and installation procedures for native Ceph client tools on Windows using the MSI installer or manual builds.
*   **install/get-packages.rst:** Central guide for configuring repositories (APT/YUM) and retrieving packages via cephadm, manual config, or direct downloads.
*   **install/index_manual.rst:** Entry point for manual installation workflows, linking to package acquisition, source cloning, and manual deployment guides.
*   **install/get-tarballs.rst:** Provides locations and instructions for downloading official Ceph source code release tarballs.
*   **install/manual-freebsd-deployment.rst:** Specific procedures for deploying Ceph on FreeBSD, focusing on ZFS pool layouts and FreeBSD-specific service management.
*   **install/install-storage-cluster.rst:** Manual package installation steps for the core Ceph storage cluster using APT and RPM, including priority configuration.
*   **install/index.rst:** High-level landing page summarizing recommended (cephadm, Rook) and alternative deployment methods.
*   **install/mirrors.rst:** List of global mirrors for Ceph packages and tarballs, including instructions for setting up a local mirror.
*   **install/manual-deployment.rst:** A step-by-step technical guide for bootstrapping a cluster manually, from FSID generation to OSD activation.
*   **install/build-ceph.rst:** Detailed instructions on build prerequisites, compiling from source using CMake/Ninja, and generating OS-specific packages.
*   **install/clone-source.rst:** Guide for cloning the Ceph git repository, managing submodules, and navigating development branches.
*   **install/windows-troubleshooting.rst:** Solutions for common Windows-specific issues, including MSI logging, WNBD driver failures, and crash dump analysis.
*   **install/windows-basic-config.rst:** Minimal configuration requirements and sample `ceph.conf` structures for Ceph clients running on Windows.

## Code Changes That Would Require Documentation Updates
*   **Dependency Shifts:** Changes to `install-deps.sh` or new required build-time/run-time libraries (e.g., changes in `cmake` requirements or `python` versions).
*   **CLI Tool Syntax:** Modifications to arguments or workflows for `cephadm`, `ceph-volume`, `monmaptool`, or `ceph-authtool`.
*   **Package Management:** Changes in the directory structure of `download.ceph.com`, repository signing keys (`release.asc`), or support for new Linux distributions (e.g., adding RHEL 9 or Ubuntu 24.04).
*   **Windows Driver/Client Updates:** Updates to the `WNBD` driver, `Dokany` version requirements, or the MSI installer build logic.
*   **Container Tagging Logic:** Shifts in how images are tagged in Quay.io or Docker Hub, or the deprecation of legacy images like `ceph/daemon`.
*   **Manual Bootstrap Workflow:** Changes to the mandatory files required for a Monitor or OSD to start (e.g., moving from `done` files to new marker files).
*   **FreeBSD Integration:** Architectural changes affecting how Ceph interacts with ZFS or the FreeBSD `rc.d` system.
*   **Default Configuration Values:** Changes to hardcoded defaults in the C++ source for `osd_pool_default_size`, `mon_host`, or network binding logic (`ms_bind_ipv6`).

## Key Technical Concepts
*   **Deployment Tools:** `cephadm`, `rook`, `ceph-ansible`, `ceph-volume`, `ceph-deploy` (deprecated).
*   **Core Daemons:** `ceph-mon` (Monitor), `ceph-osd` (Object Storage Daemon), `ceph-mds` (Metadata Server), `ceph-mgr` (Manager), `radosgw` (Gateway).
*   **Package Formats:** `DEB` (APT), `RPM` (YUM/DNF), `MSI` (Windows), `Tarball`.
*   **Configuration:** `ceph.conf`, `fsid`, `keyring`, `monmap`, `crush map`, `mon initial members`.
*   **Windows Specifics:** `WNBD`, `Dokany`, `ceph-dokan`, `ProgramData`, `AF_UNIX`.
*   **FreeBSD Specifics:** `ZFS pools`, `gpart`, `rc.conf`, `/usr/local/etc/ceph`.
*   **Build System:** `CMake`, `Ninja`, `shaman` (dev repos), `git submodules`.

## Related Components
*   **RADOS:** The underlying storage cluster.
*   **RBD (Ceph Block Device):** Specifically for the QEMU/KVM and Windows documentation.
*   **CephFS:** Related to MDS installation and Windows `ceph-dokan` usage.
*   **CephX:** The authentication system governing the creation of keyrings.
*   **CRUSH:** The algorithm managed during manual OSD addition to define data placement.