This documentation file, `storage_devices_and_osds.rst`, serves as a **design specification and gap analysis** for managing physical storage and Object Storage Daemons (OSDs) within a Ceph cluster using `cephadm`. It critiques the current state of the Orchestrator CLI and Dashboard GUI while proposing a transition from "declarative" to "imperative" management workflows.

### 1. Primary Purpose
The file documents the architectural vision and user experience (UX) improvements for storage lifecycle management. Its main goal is to shift the management model away from complex, automated "Drive Groups" toward a more predictable, user-controlled "Imperative" approach for retrieving device info, adding, removing, and replacing OSDs.

### 2. Key Topics Covered
*   **Inventory Management:** Efficiency and scalability requirements for tracking device health, blink lights (ident/fault), and usage across thousands of devices.
*   **OSD Creation Strategies:**
    *   **Device Mode:** Selecting specific devices from a global list.
    *   **Host Mode:** Creating a configuration pattern on a "base host" and replicating it across homogeneous hosts.
*   **Data Safety & Performance:** Proposals for "Conservative creation" (adding OSDs with 0 weight) to prevent massive rebalancing impact on production performance.
*   **Declarative vs. Imperative:** A core philosophical argument against "Drive Groups," citing risks like accidental disk reuse and unpredictable automation.
*   **OSD Removal/Replacement:** Safe removal workflows, including progress tracking and scheduling based on system load.

### 3. Technical Keywords
*   **APIs/Commands:** `ceph orch device ls`, `ceph orch apply osd`, `ceph orch daemon add osd`, `ceph orch osd rm`, `ceph orch device zap`.
*   **Storage Concepts:** BlueStore, WAL (Write Ahead Log), DB (Metadata database), HDD/NVMe/SSD, `ident`/`fault` LEDs, `unmanaged` parameter.
*   **Configuration:** Drive Groups, Service Specs (YAML/JSON), OSD ID preservation, weight/reweighting.
*   **Metrics:** IOPS, Raw Capacity, Fault Domains (Rack ID).

### 4. Target Audience
*   **Ceph Developers:** Specifically those working on `cephadm`, the Orchestrator module, or the Dashboard.
*   **UX/UI Designers:** To understand the mockups and proposed "Host vs. Device" modes.
*   **System Administrators:** To understand the underlying logic of how OSDs are provisioned and managed.

### 5. Related Concepts
*   **Ceph Orchestrator (`ceph-mgr`):** The underlying framework that executes these commands.
*   **BlueStore Architecture:** Relevant due to the discussion on splitting WAL/DB onto faster devices.
*   **Placement Groups (PGs):** Implicitly related through the discussion on "rebalancing" and performance impact during OSD addition/removal.
*   **Inventory/Monitoring:** Relates to how Ceph tracks hardware health (SMART data) and physical location.

---

### Triggering Updates (For AI/Automation)
Update this documentation if code changes occur in the following areas:
*   **Orchestrator Logic:** If the "Drive Group" logic is deprecated or if a new "Imperative" API is introduced.
*   **CLI Output:** Changes to the JSON structure of `ceph orch device ls` or `ceph orch osd rm status`.
*   **Dashboard Features:** Introduction of new OSD creation wizards or inventory filtering capabilities.
*   **Default Behaviors:** If the default OSD creation behavior changes (e.g., changing from "Immediate" to "Phased" reweighting).