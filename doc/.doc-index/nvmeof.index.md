# NVMEOF Documentation Index

## Overview
This documentation covers the High Availability (HA) architecture for the NVMe-over-Fabrics (NVMeof) Gateway within the Ceph ecosystem. It focuses on the implementation of Active-Standby pathing using the Asymmetric Namespace Access (ANA) protocol to ensure data consistency and continuous IO availability during gateway failures.

## Files Summary
*   **nvmeof/ha.md**: Detailed architectural overview of the High Availability design, including failover/failback mechanisms, ANA group management, load balancing strategies, and new monitor-side modules (NVMeofGwMon).

## Code Changes That Would Require Documentation Updates
*   **ANA Protocol Modifications**: Any change to how ANA states (Optimized, Inaccessible, Non-optimized) are utilized or transitioned.
*   **HA State Machine Logic**: Changes to the `MNVMeofGwMap` class or the state machine logic governing failover and failback triggers.
*   **New Monitoring Components**: Introduction of new Paxos-based monitors or modifications to `NVMeofGwMon` heartbeat/beacon intervals.
*   **Load Balancing Algorithms**: Updates to the logic that assigns namespaces to ANA groups or distributes groups across available gateways.
*   **Fencing/Blocklisting Procedures**: Changes to the Ceph blocklisting mechanism used to invalidate inflight IO from "frozen" gateways.
*   **Initialization Sequence**: Alterations to the handshake process between the Gateway and the Monitor during startup (e.g., changes to how initial ANA group IDs are assigned).
*   **CLI/gRPC Interface**: Adding or modifying commands related to manual namespace assignment or gateway management in HA mode.
*   **Timeout & Suicide Logic**: Adjustments to the "suicide" threshold or heartbeat frequency used to detect network partitions.

## Key Technical Concepts
*   **ANA (Asymmetric Namespace Access)**: Protocol used to communicate path states to the initiator.
*   **ANA Groups**: Logical collections of namespaces that share the same accessibility state on a per-gateway basis.
*   **Optimized vs. Inaccessible**: The two primary states used in this implementation to represent Active and Standby paths.
*   **Failover/Failback**: The automated processes of reassigning path ownership when a gateway disappears or reappears.
*   **NVMeofGwMon**: The Paxos-based monitor component responsible for gateway orchestration.
*   **Blocklisting**: The security mechanism used to prevent data corruption by invalidating IO from a suspected-dead gateway.
*   **Suicide Logic**: A self-termination mechanism for gateways that lose connectivity to the monitor, preventing "split-brain" scenarios.
*   **Beaconing**: The heartbeat/handshake mechanism between the Gateway Client and the Monitor.

## Related Components
*   **SPDK (Storage Performance Development Kit)**: The underlying framework for the NVMeof Gateway.
*   **Paxos**: The distributed consensus algorithm used by Ceph monitors to maintain HA state.
*   **librbd**: The Ceph block device library that the gateway interfaces with.
*   **cephadm**: The orchestration tool used to manage gateway lifecycle and removals.
*   **NVMe Initiator**: The client-side software (e.g., Linux kernel nvme driver) connecting to the gateways.