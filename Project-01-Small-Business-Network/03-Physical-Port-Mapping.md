
# Project 01 — Physical Port Mapping Document

**Client:** Northstar Business Solutions (Fictional)  
**Project:** Small Business Network Design, VLAN Segmentation & Inter-VLAN Routing  
**Environment:** Cisco Packet Tracer  
**Document Status:** Initial Physical Connectivity Verified  
**Verification Method:** Cisco Discovery Protocol (CDP)

---

## 1. Document Purpose

This document records the physical connections between the Layer 3 core switches and Layer 2 access switches in the Northstar Business Solutions network simulation.

The port mappings were verified using the Cisco IOS command:

`show cdp neighbors`

The purpose of this document is to:

- Record the actual interfaces connecting networking devices.
- Support network troubleshooting and change management.
- Establish a connectivity baseline before VLAN configuration.
- Identify primary and secondary access-switch uplinks.
- Provide evidence for the network design and implementation process.

## 2. Network Equipment Inventory

| Device Name | Model | Network Role | Location |
|---|---|---|---|
| CORE-L3-SW1 | Cisco 3650-24PS | Layer 3 Core Switch | Main Server Room |
| CORE-L3-SW2 | Cisco 3650-24PS | Layer 3 Core Switch | Main Server Room |
| GF-ACC-SW1 | Cisco 2960-24TT | Layer 2 Access Switch | Ground Floor |
| GF-ACC-SW2 | Cisco 2960-24TT | Layer 2 Access Switch | Ground Floor |
| GF-ACC-SW3 | Cisco 2960-24TT | Layer 2 Access Switch | Ground Floor |
| FF-ACC-SW1 | Cisco 2960-24TT | Layer 2 Access Switch | First Floor |
| FF-ACC-SW2 | Cisco 2960-24TT | Layer 2 Access Switch | First Floor |

## 3. Verified Physical Port Mapping

The following connections were verified using CDP on both core switches.

| Link ID | Source Device | Source Interface | Destination Device | Destination Interface |
|---|---|---|---|---|
| L01 | CORE-L3-SW1 | Gi1/0/23 | CORE-L3-SW2 | Gi1/0/23 |
| L02 | CORE-L3-SW1 | Gi1/0/1 | GF-ACC-SW1 | Gi0/1 |
| L03 | CORE-L3-SW2 | Gi1/0/1 | GF-ACC-SW1 | Gi0/2 |
| L04 | CORE-L3-SW1 | Gi1/0/2 | GF-ACC-SW2 | Gi0/1 |
| L05 | CORE-L3-SW2 | Gi1/0/2 | GF-ACC-SW2 | Gi0/2 |
| L06 | CORE-L3-SW1 | Gi1/0/3 | GF-ACC-SW3 | Gi0/1 |
| L07 | CORE-L3-SW2 | Gi1/0/3 | GF-ACC-SW3 | Gi0/2 |
| L08 | CORE-L3-SW1 | Gi1/0/4 | FF-ACC-SW1 | Gi0/1 |
| L09 | CORE-L3-SW2 | Gi1/0/4 | FF-ACC-SW1 | Gi0/2 |
| L10 | CORE-L3-SW1 | Gi1/0/5 | FF-ACC-SW2 | Gi0/1 |
| L11 | CORE-L3-SW2 | Gi1/0/5 | FF-ACC-SW2 | Gi0/2 |

**Total:** 11 physical connections, consisting of 10 access-switch uplinks and 1 inter-core connection.

## 4. Redundancy Design

Each access switch is connected to both Layer 3 core switches.

This provides an alternative physical network path if one core uplink or core switch fails.

The inter-core connection provides a direct link between the two Layer 3 switches.

### Important Considerations

- Redundant physical links do not automatically guarantee uninterrupted connectivity.
- Spanning Tree Protocol (STP) prevents Layer 2 switching loops.
- Some redundant ports may be placed into an STP blocking state.
- VRRP will be evaluated for default gateway redundancy.
- VLAN trunking, STP behaviour and failover must be verified during implementation.

## 5. Physical Cabling

The current Packet Tracer simulation uses:

| Connection Type | Simulated Cable |
|---|---|
| Core Switch to Core Switch | Copper Cross-Over |
| Core Switch to Access Switch | Copper Straight-Through |

These are simulation cabling choices.

For a physical deployment, inter-floor fibre-optic connections remain under evaluation, subject to distance, pathway, transceiver compatibility and cost.

## 6. Verification Results

### CORE-L3-SW1

**Command:** `show cdp neighbors`

Result:

- CORE-L3-SW2 discovered.
- GF-ACC-SW1 discovered.
- GF-ACC-SW2 discovered.
- GF-ACC-SW3 discovered.
- FF-ACC-SW1 discovered.
- FF-ACC-SW2 discovered.

**Status:** Six expected neighbours discovered.

### CORE-L3-SW2

**Command:** `show cdp neighbors`

Result:

- CORE-L3-SW1 discovered.
- GF-ACC-SW1 discovered.
- GF-ACC-SW2 discovered.
- GF-ACC-SW3 discovered.
- FF-ACC-SW1 discovered.
- FF-ACC-SW2 discovered.

**Status:** Six expected neighbours discovered.

### Verification Limitations

CDP confirms neighbour discovery and interface mappings.

It does not confirm:

- Correct VLAN assignments.
- Operational VLAN trunking.
- Inter-VLAN routing.
- VRRP failover.
- End-to-end application connectivity.

These will be verified during later implementation stages.

## 7. Next Steps

Before configuring the network:

1. Review and validate the physical topology.
2. Prepare the Method of Procedure (MOP).
3. Document pre-implementation checks.
4. Plan VLAN and trunk configurations.
5. Plan STP and gateway redundancy configurations.
6. Define verification and rollback procedures.

---

## Disclaimer

This document describes a fictional network implemented in Cisco Packet Tracer for educational and portfolio development purposes.

Physical connections and CDP results relate to the simulated environment, not a production network.
