This documentation file, `install/get-packages.rst`, serves as the foundational guide for acquiring Ceph software across various operating systems and deployment scenarios.

### 1. Primary Purpose
The file documents the multiple methodologies for retrieving Ceph installation packages, configuring the necessary software repositories (APT/YUM/ZYPPER), and verifying package integrity using GPG keys. It ensures users can access stable, development, or specific version-locked releases.

### 2. Key Topics Covered
*   **Acquisition Methods**:
    *   **Cephadm**: Automated repository configuration using the `cephadm` tool.
    *   **Manual Repository Configuration**: Step-by-step instructions for adding official Ceph sources to system package managers.
    *   **Manual Download**: Instructions for offline or firewalled environments.
*   **Security**: Verification of packages using the `release.asc` GPG key.
*   **Release Tiers**: Instructions for Major Releases (Stable), Testing/QA builds, and specific Development branches.
*   **Platform Specifics**: Detailed configuration for Debian/Ubuntu (DEB), RHEL/CentOS (RPM), openSUSE, and openEuler.
*   **Development Builds**: Integration with the **Shaman** API for retrieving the latest builds from specific Git branches/SHA1s.

### 3. Technical Keywords
*   **Tools/Commands**: `cephadm`, `apt-add-repository`, `rpm --import`, `zypper ar`, `wget`, `curl`, `yum`.
*   **Configuration Files**: `/etc/apt/sources.list.d/`, `/etc/yum.repos.d/ceph.repo`.
*   **APIs/Services**: `download.ceph.com`, `shaman.ceph.com` (build system API).
*   **Package Types**: `.deb`, `.rpm`, `noarch`, `SRPMS`.
*   **Distribution Codes**: `el8`, `el9`, `xenial`, `lsb_release -sc`.
*   **Ceph Versions**: Octopus (15.2.z), Luminous, Mimic, Nautilus.

### 4. Target Audience
*   **System Administrators**: Setting up production or staging Ceph clusters.
*   **DevOps Engineers**: Automating cluster deployments via scripts or configuration management.
*   **Ceph Developers**: Testing specific feature branches or bug fixes.
*   **Security/Network Compliance Officers**: Managing package installation in air-gapped or restricted environments.

### 5. Related Concepts
*   **Cephadm**: The primary management tool that often abstracts these steps.
*   **Ceph Releases**: The lifecycle management of stable vs. development versions.
*   **Mirroring**: Using local mirrors to reduce latency and bandwidth.
*   **EPEL (Extra Packages for Enterprise Linux)**: Required dependencies for RHEL-based systems.

---

### Triggering Updates: When to Edit This File
An AI or maintainer should update this file if any of the following code or infrastructure changes occur:

1.  **New Stable Releases**: When a new major version (e.g., "Quincy" or "Reef") is released, the `|stable-release|` variables and example URLs must be updated.
2.  **OS Support Changes**: If Ceph adds support for a new Linux distribution (e.g., a new RHEL major version) or drops support for an EOL distribution (e.g., Ubuntu 16.04).
3.  **Tooling Evolution**: If the `cephadm` CLI syntax for `add-repo` or `install` changes.
4.  **Infrastructure Shifts**: If the primary download domain (`download.ceph.com`) or the development build API (`shaman.ceph.com`) changes their URL structures or authentication methods.
5.  **Security Policy**: Changes to GPG signing procedures or the location of the `release.asc` key.