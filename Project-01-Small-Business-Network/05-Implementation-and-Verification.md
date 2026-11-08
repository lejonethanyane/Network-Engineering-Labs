
# 05 — Implementation and Verification

## Project Information

**Project:** Northstar Business Solutions — Small Business Network

**Environment:** Cisco Packet Tracer (Simulated Lab)

**Implementation Type:** Simulated Network Implementation and Security Validation

**Pre-ACL Checkpoint:** Northstar-Project01-Pre-ACL-v09.pkt

**Post-ACL Checkpoint:** Northstar-Project01-ACL-Validated-v10.pkt

## 1. Implementation Overview

This document records the configuration, verification, troubleshooting and security testing performed during the implementation of the Northstar Business Solutions network.

The implementation includes:

- VLAN creation and segmentation
- Redundant core and access-switch trunk connections
- Spanning Tree Protocol (STP) configuration
- Access-port configuration and unused-port shutdown
- Inter-VLAN routing using Layer 3 switches
- HSRP gateway redundancy for VLANs 10 and 20
- Endpoint addressing and connectivity testing
- Sales-to-Finance access restrictions
- Guest VLAN isolation
- Post-implementation verification and rollback considerations

All results were obtained in a simulated Cisco Packet Tracer environment and should not be interpreted as production deployment validation.


## 2. VLAN Implementation and Verification

### 2.1 Implementation Objective

Implement VLAN segmentation to separate business departments, infrastructure services and guest devices into dedicated Layer 2 broadcast domains.

Nine VLANs were defined for the simulated Northstar Business Solutions network.

### 2.2 Implemented VLANs

| VLAN ID | VLAN Name | Network Address | Subnet Mask | Purpose |
|---|---|---|---|---|
| 10 | SALES | 192.168.10.0/27 | 255.255.255.224 | Sales workstations |
| 20 | OPERATIONS | 192.168.20.0/26 | 255.255.255.192 | Operations workstations |
| 30 | FINANCE | 192.168.30.0/27 | 255.255.255.224 | Finance workstations |
| 40 | IT | 192.168.40.0/28 | 255.255.255.240 | IT administration workstations |
| 50 | SERVERS | 192.168.50.0/27 | 255.255.255.224 | Internal servers |
| 60 | VOICE | 192.168.60.0/26 | 255.255.255.192 | Voice devices |
| 70 | CCTV | 192.168.70.0/28 | 255.255.255.240 | CCTV devices |
| 80 | GUEST | 192.168.80.0/27 | 255.255.255.224 | Guest devices |
| 90 | MANAGEMENT | 192.168.90.0/27 | 255.255.255.224 | Network device management |

### 2.3 Devices Configured

VLAN implementation was performed across the following devices:

**Layer 3 Core Switches:**
- CORE-L3-SW1
- CORE-L3-SW2

**Ground-Floor Access Switches:**
- GF-ACC-SW1
- GF-ACC-SW2
- GF-ACC-SW3

**First-Floor Access Switches:**
- FF-ACC-SW1
- FF-ACC-SW2

### 2.4 Verification Procedures

The following Cisco IOS commands were used during implementation and validation:

```cisco
show vlan brief
show interfaces trunk
show interfaces status
```

The verification process included:

1. Confirming that VLAN IDs and names appeared correctly.
2. Verifying that configured endpoint ports belonged to their intended VLANs.
3. Confirming that inter-switch uplinks operated as 802.1Q trunks.
4. Confirming that the planned VLANs were allowed across the trunk connections.
5. Verifying that unused access ports were administratively disabled.

### 2.5 Verification Results

The VLAN configuration was successfully implemented in the Packet Tracer environment.

Observed results included:

- The nine planned VLANs appeared in switch VLAN tables.
- Connected Sales, Operations, Finance, IT, Server and Guest endpoints were assigned to their intended access VLANs.
- The configured access-switch uplinks operated as trunks.
- Unused access ports were administratively disabled.
- Subsequent endpoint connectivity tests demonstrated communication within VLAN 20 across both floors.

  
## 3. Spanning Tree Protocol (STP) Implementation and Verification

### 3.1 Implementation Objective

Configure and verify Spanning Tree Protocol (STP) to prevent Layer 2 switching loops while maintaining redundant connections between the core and access switches.

The Northstar Business Solutions topology contains two Layer 3 core switches and five access switches.

Each access switch has an uplink to both core switches, providing alternative physical network paths.

### 3.2 STP Implementation

STP bridge priorities were configured on the core switches to influence root bridge elections.

The configuration was designed to keep root bridge selection predictable rather than relying only on the default switch priorities and MAC addresses.

STP remained enabled throughout implementation.

### 3.3 Verification Commands

The following Cisco IOS commands were used to inspect STP operation:

```cisco
show spanning-tree vlan 1
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree inconsistentports
```

Additional VLAN-specific STP checks are required as part of the final network validation.

### 3.4 Observed STP Results

Earlier Packet Tracer CLI outputs provided the following evidence:

| VLAN | Observed Root Bridge | Root ID Priority |
|---|---|---|
| 10 — SALES | CORE-L3-SW1 | 4106 |
| 20 — OPERATIONS | CORE-L3-SW2 | 4116 |

The displayed Root ID priorities include the VLAN system-ID extension.

Access-switch verification showed different port roles, including:

- Root ports in the forwarding state.
- Alternate ports in the blocking state.
- Different forwarding paths for VLAN 10 and VLAN 20.

The alternate blocking state is expected in a redundant Layer 2 topology. It prevents switching loops while retaining an alternative path.

During verification on FF-ACC-SW2, the `show spanning-tree inconsistentports` command reported zero inconsistent ports.

### 3.5 Implementation and Verification Status

The recorded results demonstrated that STP was operating and preventing Layer 2 loops for the VLANs inspected.

**Verified:** VLAN 10 and VLAN 20 root bridge information and access-switch port roles.

**Additional verification required:** Confirm the current root bridge and port roles for all remaining VLANs after the complete network configuration.

**STP Verification Status:** Operational for VLANs 10 and 20. Final verification for the remaining VLANs is pending.

**Environment:** Cisco Packet Tracer simulation.


## 4. Trunk Configuration and Verification

### 4.1 Implementation Objective

Configure 802.1Q trunk connections between the Layer 3 core switches and Layer 2 access switches to transport multiple VLANs across the network.

The network was designed with redundant physical uplinks to provide alternative paths if a connection becomes unavailable.

The implemented topology contains:

- Two Layer 3 core switches.
- Five Layer 2 access switches.
- Ten core-to-access-switch uplinks.
- One inter-core switch connection.
- Eleven inter-switch physical connections in total.

### 4.2 Physical Trunk Port Mapping

The following interface connections were recorded and verified using Cisco Discovery Protocol (CDP).

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

All five access switches have two physical uplinks, one to each core switch.

### 4.3 Trunk Configuration

The inter-switch links were configured to operate as 802.1Q trunks.

The trunk configuration allows the following VLANs:

`1,10,20,30,40,50,60,70,80,90`

VLAN 1 was retained for compatibility with the existing lab configuration. It is not used for departmental endpoint traffic.

The native VLAN remained at its default value of VLAN 1 during this simulation.

Example trunk configuration:

```cisco
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30,40,50,60,70,80,90
```

The configuration shown is representative of an access-switch uplink. Interface numbers differ between the core and access switches.

### 4.4 Trunk Verification Commands

The following commands were used during configuration and verification:

```cisco
show interfaces trunk
show interfaces status
show cdp neighbors
show interfaces gigabitEthernet0/1 switchport
show spanning-tree vlan 10
show spanning-tree vlan 20
```

Verification included checking:

1. Whether trunk interfaces were operational.
2. Whether 802.1Q encapsulation was in use.
3. Whether the planned VLANs were included in the allowed VLAN list.
4. Whether the connected neighbouring switches matched the physical port mapping.
5. Whether STP was controlling redundant Layer 2 forwarding paths.

### 4.5 Observed Verification Results

Packet Tracer CLI outputs confirmed the following:

- The configured access-switch uplinks operated as trunks.
- Both uplinks on the inspected access switches were physically connected.
- The trunk configuration allowed VLANs 1, 10, 20, 30, 40, 50, 60, 70, 80 and 90.
- The VLANs were shown as allowed and active in the management domain.
- CDP was used to verify inter-switch connections and interface assignments.
- STP placed some redundant ports into alternate blocking states to prevent Layer 2 switching loops.

**Important:** A physically connected trunk does not necessarily forward traffic for every VLAN. STP can block a trunk for a particular VLAN while allowing another path to forward traffic.

### 4.6 Redundancy and Outstanding Testing

The physical topology provides alternative connections between access switches and the core infrastructure.

However, physical redundancy alone does not prove successful failover.

Further testing is required to confirm:

- Traffic behaviour when an active uplink is disconnected.
- STP convergence following a link failure.
- Restoration of connectivity after the failed link is reconnected.
- Whether all required VLANs remain reachable during a failure.

These tests will be recorded separately during the network resilience verification phase.

**Trunk Configuration Status:** Implemented and basic connectivity verified.

**Failover Verification Status:** Pending controlled link-failure testing.

**Environment:** Cisco Packet Tracer simulation.


## 5. Access Port Configuration and Verification

### 5.1 Implementation Objective

Configure access-switch ports to place connected endpoint devices into their designated VLANs.

Interface descriptions were added to identify the intended purpose of each port.

Unused access ports were administratively disabled to reduce the risk of unauthorised devices connecting to the network.

### 5.2 Configured Access Port Assignments

The following access-port assignments were implemented in the Packet Tracer environment.

| Switch | Interface | VLAN | Description | Endpoint / Status |
|---|---|---|---|---|
| GF-ACC-SW1 | Fa0/1 | 10 | SALES_PC | GF-SALES-PC1 — Connected |
| GF-ACC-SW1 | Fa0/2 | 20 | OPERATIONS_PC | GF-OPERATIONS-PC1 — Connected |
| GF-ACC-SW1 | Fa0/3 | 10 / Voice 60 | SALES_PHONE_AND_PC | Reserved — No endpoint connected |
| GF-ACC-SW2 | Fa0/1 | 80 | GUEST_TEST_PC | GF-GUEST-PC1 — Connected |
| GF-ACC-SW3 | Fa0/1 | 40 | IT_PC | GF-IT-PC1 — Connected |
| GF-ACC-SW3 | Fa0/2 | 50 | TEST_SERVER | GF-TEST-SERVER1 — Connected |
| GF-ACC-SW3 | Fa0/3 | 70 | CCTV_TEST_DEVICE | Reserved — No endpoint connected |
| FF-ACC-SW1 | Fa0/1 | 30 | FINANCE_PC | FF-FINANCE-PC1 — Connected |
| FF-ACC-SW2 | Fa0/1 | 20 | OPERATIONS_PC | FF-OPERATIONS-PC1 — Connected |

All connected endpoints were assigned static IP addresses from their corresponding VLAN subnets.

The reserved Sales phone port and CCTV port have not yet been tested with their intended endpoint devices.

### 5.3 Access Port Configuration

Access ports were configured with static VLAN assignments.

Example configuration for the Sales workstation on GF-ACC-SW1:

```cisco
interface FastEthernet0/1
 description SALES_PC
 switchport mode access
 switchport access vlan 10
 no shutdown
```

This configuration places the connected Sales workstation into VLAN 10.

Other endpoint ports were configured using the same approach, with the VLAN ID and description changed according to their purpose.

The planned Sales phone port additionally has Voice VLAN 60 configured. Voice endpoint functionality requires separate verification.

### 5.4 Unused Port Security

Unused access ports were labelled and administratively disabled.

Example configuration:

```cisco
interface range FastEthernet0/4-24
 description UNUSED_RESERVED
 shutdown
```

The example applies to GF-ACC-SW1, where Fa0/1 through Fa0/3 were reserved for planned devices.

Other access switches used different unused-port ranges depending on their active and reserved interface assignments.

The objective was to ensure unused ports did not remain operational unnecessarily.

### 5.5 Verification Commands

The following Cisco IOS commands were used to verify access-port configuration:

```cisco
show interfaces status
show vlan brief
show interfaces fastEthernet0/1 switchport
show running-config
```

Verification included:

1. Confirming that connected endpoint ports displayed the correct VLAN ID.
2. Checking that access ports operated in static access mode.
3. Confirming that interface descriptions matched the intended devices.
4. Verifying that connected endpoints had operational physical links.
5. Confirming that unused reserved ports were administratively disabled.
6. Ensuring the GigabitEthernet uplink interfaces remained configured as trunks rather than access ports.

### 5.6 Observed Verification Results

Packet Tracer CLI verification confirmed that:

- GF-SALES-PC1 was connected to GF-ACC-SW1 Fa0/1 in VLAN 10.
- Ground-floor and first-floor Operations PCs were connected to VLAN 20 access ports.
- FF-FINANCE-PC1 was connected to FF-ACC-SW1 Fa0/1 in VLAN 30.
- GF-IT-PC1 was connected to GF-ACC-SW3 Fa0/1 in VLAN 40.
- GF-TEST-SERVER1 was connected to GF-ACC-SW3 Fa0/2 in VLAN 50.
- GF-GUEST-PC1 was connected to GF-ACC-SW2 Fa0/1 in VLAN 80.
- The connected endpoint interfaces operated at 100 Mbps, full duplex.
- Unused reserved access ports displayed an administratively disabled status.
- Both core-switch uplinks remained connected on the inspected access switches.

### 5.7 Outstanding Verification

The following access-port tests remain outstanding:

- Connect and verify a device using the planned Voice VLAN 60 configuration.
- Connect and verify the CCTV test endpoint in VLAN 70.
- Review whether additional access-port security controls are required for a production design.

**Access Port Implementation Status:** Completed for the currently connected lab endpoints.

**Reserved Endpoint Verification Status:** Voice and CCTV endpoint testing pending.

**Environment:** Cisco Packet Tracer simulation.


## 6. Inter-VLAN Routing and SVI Configuration

### 6.1 Implementation Objective

Enable communication between authorised VLANs using the two Layer 3 core switches.

Switch Virtual Interfaces (SVIs) were configured to provide Layer 3 connectivity for each departmental and infrastructure VLAN.

IP routing was enabled on both core switches, allowing traffic to be routed between directly connected VLAN networks.

Inter-VLAN routing was configured before implementing ACL restrictions to establish a working connectivity baseline.

### 6.2 SVI IP Addressing Plan

Each core switch was assigned a unique SVI IP address within every VLAN subnet.

| VLAN | Network | Subnet Mask | CORE-L3-SW1 | CORE-L3-SW2 |
|---|---|---|---|---|
| 10 — SALES | 192.168.10.0/27 | 255.255.255.224 | 192.168.10.2 | 192.168.10.3 |
| 20 — OPERATIONS | 192.168.20.0/26 | 255.255.255.192 | 192.168.20.2 | 192.168.20.3 |
| 30 — FINANCE | 192.168.30.0/27 | 255.255.255.224 | 192.168.30.2 | 192.168.30.3 |
| 40 — IT | 192.168.40.0/28 | 255.255.255.240 | 192.168.40.2 | 192.168.40.3 |
| 50 — SERVERS | 192.168.50.0/27 | 255.255.255.224 | 192.168.50.2 | 192.168.50.3 |
| 60 — VOICE | 192.168.60.0/26 | 255.255.255.192 | 192.168.60.2 | 192.168.60.3 |
| 70 — CCTV | 192.168.70.0/28 | 255.255.255.240 | 192.168.70.2 | 192.168.70.3 |
| 80 — GUEST | 192.168.80.0/27 | 255.255.255.224 | 192.168.80.2 | 192.168.80.3 |
| 90 — MANAGEMENT | 192.168.90.0/27 | 255.255.255.224 | 192.168.90.2 | 192.168.90.3 |

The `.1` address in each subnet was reserved for the planned first-hop redundancy virtual gateway.

HSRP was implemented and tested for VLANs 10 and 20. The remaining VLANs continued to use individual core-switch SVI addresses as temporary default gateways.

### 6.3 Layer 3 Routing Configuration

The `ip routing` command was enabled on both Layer 3 core switches.

Example configuration for CORE-L3-SW1:

```cisco
configure terminal
ip routing

interface Vlan10
 ip address 192.168.10.2 255.255.255.224
 no shutdown

interface Vlan20
 ip address 192.168.20.2 255.255.255.192
 no shutdown
```

The remaining SVIs were configured using their corresponding VLAN IDs, IP addresses and subnet masks.

CORE-L3-SW2 was configured with the `.3` SVI addresses.

Each SVI provides a Layer 3 interface through which the switch can route traffic to and from the associated VLAN.

### 6.4 Routing Verification Commands

The following commands were used to inspect SVI status and routing information:

```cisco
show ip interface brief
show ip route
show running-config
```

Verification included:

1. Confirming that the planned SVIs existed on both core switches.
2. Checking that the SVIs had the correct IP addresses and subnet masks.
3. Confirming that operational VLAN interfaces displayed an up/up status.
4. Checking that connected VLAN networks appeared in the IP routing table.
5. Confirming that individual SVI IP addresses appeared as local routes.

### 6.5 Observed Routing Table Results

The `show ip route` command displayed connected and local routes for the configured VLAN networks.

Examples of route types observed:

```text
C 192.168.10.0/27 is directly connected, Vlan10
L 192.168.10.2/32 is directly connected, Vlan10

C 192.168.20.0/26 is directly connected, Vlan20
L 192.168.20.2/32 is directly connected, Vlan20
```

**C — Connected Route:** Identifies a network directly connected to the Layer 3 switch.

**L — Local Route:** Identifies the specific IP address assigned to the switch interface.

The routing tables showed that the core switches recognised their configured VLAN subnets.

No default route to an upstream internet router was configured or verified during this phase.

### 6.6 Inter-VLAN Connectivity Verification

The following tests were performed before applying the network security ACLs.

| Source Device | Source VLAN | Destination | Destination VLAN | Result |
|---|---|---|---|---|
| GF-SALES-PC1 | 10 | GF-OPERATIONS-PC1 | 20 | Passed |
| GF-SALES-PC1 | 10 | FF-FINANCE-PC1 | 30 | Passed |
| GF-IT-PC1 | 40 | FF-FINANCE-PC1 | 30 | Passed |
| GF-TEST-SERVER1 | 50 | GF-IT-PC1 | 40 | Passed |
| GF-SALES-PC1 | 10 | GF-TEST-SERVER1 | 50 | Passed |
| GF-GUEST-PC1 | 80 | FF-FINANCE-PC1 | 30 | Passed before ACL |

The successful ping tests demonstrated that inter-VLAN routing was functioning between the tested networks.

Some initial ping attempts experienced packet loss before subsequent tests succeeded. Follow-up tests confirmed connectivity.

After ACL implementation, certain previously successful connections were intentionally blocked according to the security policy. Those results will be documented in the ACL verification section.

### 6.7 Same-VLAN Connectivity Between Floors

A separate test was performed to verify connectivity between two Operations workstations in VLAN 20.

| Device | Location | IP Address | VLAN |
|---|---|---|---|
| GF-OPERATIONS-PC1 | Ground Floor | 192.168.20.10 | 20 |
| FF-OPERATIONS-PC1 | First Floor | 192.168.20.11 | 20 |

Bidirectional ping tests completed successfully with 0% packet loss.

Because the workstations belong to the same subnet, their communication used Layer 2 switching rather than inter-VLAN routing.

This provided additional evidence that VLAN 20 connectivity was functioning across the inter-switch trunk infrastructure.

### 6.8 Implementation Status and Outstanding Testing

The following functionality was implemented and verified:

- IP routing enabled on both Layer 3 core switches.
- SVI addressing for VLANs 10 through 90.
- Connected and local routes present in the routing tables.
- Inter-VLAN communication between tested endpoints before ACL enforcement.
- Same-VLAN connectivity between Operations workstations on different floors.

Outstanding work includes:

- Completing HSRP gateway redundancy for the remaining VLANs.
- Verifying routing behaviour during core-switch and uplink failures.
- Adding and testing additional endpoints in the Voice, CCTV and Management VLANs.
- Implementing and validating upstream internet routing.

**Inter-VLAN Routing Status:** Implemented and verified for the tested VLANs.

**Remaining Gateway Redundancy Status:** Pending for VLANs other than 10 and 20.

**Environment:** Cisco Packet Tracer simulation.


## 7. HSRP Gateway Redundancy Implementation and Verification

### 7.1 Implementation Objective

Configure first-hop gateway redundancy between CORE-L3-SW1 and CORE-L3-SW2 using Hot Standby Router Protocol (HSRP).

The objective was to provide a shared virtual default gateway and allow the standby core switch to assume the active gateway role if the preferred core switch became unavailable.

HSRP was implemented and tested for:

- VLAN 10 — SALES
- VLAN 20 — OPERATIONS

The remaining VLANs are not yet configured for HSRP.

### 7.2 HSRP Addressing and Gateway Roles

Both core switches have unique SVI addresses, while the HSRP group provides a shared virtual IP address for hosts.

| VLAN | HSRP Group | Virtual IP | Preferred Active Switch | Active Priority | Standby Priority |
|---|---|---|---|---|---|
| 10 — SALES | 10 | 192.168.10.1 | CORE-L3-SW1 | 110 | 100 |
| 20 — OPERATIONS | 20 | 192.168.20.1 | CORE-L3-SW2 | 110 | 100 |

The preferred active and standby roles were distributed between the two core switches.

For VLAN 10:

- CORE-L3-SW1 SVI: 192.168.10.2 — Preferred Active
- CORE-L3-SW2 SVI: 192.168.10.3 — Standby
- HSRP virtual gateway: 192.168.10.1

For VLAN 20:

- CORE-L3-SW1 SVI: 192.168.20.2 — Standby
- CORE-L3-SW2 SVI: 192.168.20.3 — Preferred Active
- HSRP virtual gateway: 192.168.20.1

### 7.3 HSRP Configuration

The following configuration was implemented on CORE-L3-SW1:

```cisco
interface Vlan10
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt

interface Vlan20
 standby 20 ip 192.168.20.1
```

The following configuration was implemented on CORE-L3-SW2:

```cisco
interface Vlan10
 standby 10 ip 192.168.10.1

interface Vlan20
 standby 20 ip 192.168.20.1
 standby 20 priority 110
 standby 20 preempt
```

The default HSRP priority is 100.

The `preempt` command was configured on each preferred active gateway so that it could reclaim the active role after recovering, provided its priority remained higher.

### 7.4 Verification Commands

The following commands were used during configuration and testing:

```cisco
show standby brief
show ip interface brief
show running-config
```

HSRP verification included checking:

1. The configured HSRP group number.
2. The virtual IP address assigned to each group.
3. The active and standby gateway roles.
4. The configured priorities.
5. Whether preemption was enabled on the preferred active switch.
6. Whether both SVIs were operational after the failover tests.

### 7.5 Observed Normal Operating State

Before failure simulation, the verified HSRP roles were:

| VLAN | CORE-L3-SW1 | CORE-L3-SW2 | Virtual Gateway |
|---|---|---|---|
| 10 — SALES | Active | Standby | 192.168.10.1 |
| 20 — OPERATIONS | Standby | Active | 192.168.20.1 |

The `show standby brief` command displayed the active and standby gateway information.

The HSRP state outputs also confirmed that each switch recognised the opposite core switch as its HSRP peer.

### 7.6 Controlled HSRP Failover Testing

Two failover scenarios were tested by administratively shutting down the SVI on the preferred active gateway.

#### Test 1 — VLAN 10 Gateway Failover

**Initial State:**
- CORE-L3-SW1: Active
- CORE-L3-SW2: Standby

**Test Procedure:**

The VLAN 10 SVI was administratively shut down on CORE-L3-SW1.

```cisco
interface Vlan10
 shutdown
```

**Observed Result:**

CORE-L3-SW2 transitioned to the Active HSRP role for VLAN 10.

The interface was then restored on CORE-L3-SW1:

```cisco
interface Vlan10
 no shutdown
```

With priority 110 and preemption enabled, CORE-L3-SW1 reclaimed the Active role after recovery.

**Control-Plane Failover Result:** Passed.

#### Test 2 — VLAN 20 Gateway Failover

**Initial State:**
- CORE-L3-SW1: Standby
- CORE-L3-SW2: Active

**Test Procedure:**

The VLAN 20 SVI was administratively shut down on CORE-L3-SW2.

```cisco
interface Vlan20
 shutdown
```

**Observed Result:**

CORE-L3-SW1 transitioned to the Active HSRP role for VLAN 20.

The interface was then restored on CORE-L3-SW2:

```cisco
interface Vlan20
 no shutdown
```

With priority 110 and preemption enabled, CORE-L3-SW2 reclaimed the Active role after recovery.

**Control-Plane Failover Result:** Passed.

### 7.7 Final HSRP Verification

Following restoration of the SVIs, the observed HSRP roles returned to the intended design:

- VLAN 10: CORE-L3-SW1 Active, CORE-L3-SW2 Standby.
- VLAN 20: CORE-L3-SW2 Active, CORE-L3-SW1 Standby.

The test results demonstrated successful HSRP role changes and recovery during the simulated SVI failures.

The tests verified HSRP control-plane behaviour. Continuous endpoint traffic and application availability were not measured during the failover events.

### 7.8 Outstanding HSRP and Resilience Testing

The following work remains outstanding:

- Configure and verify HSRP for the remaining VLANs where redundancy is required.
- Verify endpoint connectivity continuously during gateway failover.
- Measure packet loss and approximate recovery time during simulated failures.
- Test physical uplink failures separately from SVI shutdown scenarios.
- Verify that the ACL security policies remain effective during HSRP failover.
- Confirm restoration of normal forwarding paths after recovery.

HSRP provides first-hop gateway redundancy, while STP manages redundant Layer 2 paths. Both technologies must operate correctly for overall network resilience.

**HSRP Configuration Status:** Implemented for VLANs 10 and 20.

**HSRP Control-Plane Failover Status:** Passed for both tested VLANs.

**End-to-End Failover Performance Verification:** Pending.

**Environment:** Cisco Packet Tracer simulation.


## 8. ACL Security Implementation and Verification

### 8.1 Implementation Objective

Implement extended IP Access Control Lists (ACLs) to restrict unauthorised communication between selected VLANs while preserving permitted network connectivity.

The implementation focused on two security requirements:

1. Prevent Sales workstations in VLAN 10 from accessing the Finance network in VLAN 30.
2. Prevent Guest devices in VLAN 80 from accessing the defined internal company VLANs.

Identical ACL policies were configured on both Layer 3 core switches to maintain consistent restrictions when traffic uses either routing device.

Security testing was performed before and after applying the ACLs.

### 8.2 Security Policies Implemented

| Source VLAN | Destination | Required Action | Policy |
|---|---|---|---|
| 10 — SALES | 30 — FINANCE | Deny | Finance protection |
| 10 — SALES | 50 — SERVERS | Permit | Preserve permitted connectivity |
| 40 — IT | 30 — FINANCE | Permit | Preserve authorised IT access |
| 80 — GUEST | 10 — SALES | Deny | Guest isolation |
| 80 — GUEST | 20 — OPERATIONS | Deny | Guest isolation |
| 80 — GUEST | 30 — FINANCE | Deny | Guest isolation |
| 80 — GUEST | 40 — IT | Deny | Guest isolation |
| 80 — GUEST | 50 — SERVERS | Deny | Guest isolation |
| 80 — GUEST | 60 — VOICE | Deny | Guest isolation |
| 80 — GUEST | 70 — CCTV | Deny | Guest isolation |
| 80 — GUEST | 90 — MANAGEMENT | Deny | Guest isolation |

The policies were designed for the currently defined lab subnets.

### 8.3 Sales-to-Finance ACL Configuration

The following extended ACL was configured on CORE-L3-SW1 and CORE-L3-SW2:

```cisco
ip access-list extended SALES_BLOCK_FINANCE
 deny ip 192.168.10.0 0.0.0.31 192.168.30.0 0.0.0.31
 permit ip any any
```

The ACL was applied inbound on the VLAN 10 SVI of both core switches:

```cisco
interface Vlan10
 ip access-group SALES_BLOCK_FINANCE in
```

The deny statement blocks IP traffic originating from the Sales subnet and destined for the Finance subnet.

The final permit statement allows other unmatched IP traffic.

Applying the ACL inbound on both VLAN 10 SVIs ensures that either core switch can enforce the configured Sales restriction when routing traffic from that VLAN.

### 8.4 Sales-to-Finance Verification Results

Before applying the ACL, GF-SALES-PC1 successfully pinged FF-FINANCE-PC1.

After applying the ACL, the same test was repeated.

| Test | Before ACL | After ACL | Required Result |
|---|---|---|---|
| Sales → Finance | 4/4 replies | 0/4 replies | Blocked |
| Sales → Server | 4/4 replies | 4/4 replies | Allowed |
| IT → Finance | 4/4 replies | 4/4 replies | Allowed |

The blocked Sales-to-Finance test returned:

```text
Reply from 192.168.10.2: Destination host unreachable.
```

This was consistent with CORE-L3-SW1 denying the traffic.

The successful Sales-to-Server and IT-to-Finance tests confirmed that the intended permitted communications remained operational.

**Sales-to-Finance ACL Verification Status:** Passed for the tested traffic.

### 8.5 Guest Isolation ACL Configuration

The following ACL was created on both core switches:

```cisco
ip access-list extended GUEST_ISOLATION
 deny ip 192.168.80.0 0.0.0.31 192.168.10.0 0.0.0.31
 deny ip 192.168.80.0 0.0.0.31 192.168.20.0 0.0.0.63
 deny ip 192.168.80.0 0.0.0.31 192.168.30.0 0.0.0.31
 deny ip 192.168.80.0 0.0.0.31 192.168.40.0 0.0.0.15
 deny ip 192.168.80.0 0.0.0.31 192.168.50.0 0.0.0.31
 deny ip 192.168.80.0 0.0.0.31 192.168.60.0 0.0.0.63
 deny ip 192.168.80.0 0.0.0.31 192.168.70.0 0.0.0.15
 deny ip 192.168.80.0 0.0.0.31 192.168.90.0 0.0.0.31
 permit ip any any
```

The ACL was applied inbound on VLAN 80 on both core switches:

```cisco
interface Vlan80
 ip access-group GUEST_ISOLATION in
```

The eight deny statements restrict Guest traffic destined for the defined internal VLAN subnets.

The final permit statement allows unmatched traffic. This does not establish internet access, because upstream internet routing and firewall connectivity have not yet been implemented.

### 8.6 Guest Isolation Verification — CORE-L3-SW1

The Guest workstation was configured with:

- Device: GF-GUEST-PC1
- IP address: 192.168.80.10/27
- Default gateway: 192.168.80.2
- Routing device: CORE-L3-SW1

The following ping tests were performed after applying the Guest isolation ACL.

| Destination | Destination IP | Result |
|---|---|---|
| Finance PC | 192.168.30.10 | Blocked — 0/4 replies |
| Test Server | 192.168.50.10 | Blocked — 0/4 replies |
| Sales PC | 192.168.10.10 | Blocked — 0/4 replies |
| IT PC | 192.168.40.10 | Blocked — 0/4 replies |
| Operations PC | 192.168.20.10 | Blocked — 0/4 replies |
| Management SVI | 192.168.90.2 | Blocked — 0/4 replies |
| Voice SVI | 192.168.60.2 | Blocked — 0/4 replies |
| CCTV SVI | 192.168.70.2 | Blocked — 0/4 replies |
| Guest Gateway | 192.168.80.2 | Allowed — 4/4 replies |

Blocked tests returned messages similar to:

```text
Reply from 192.168.80.2: Destination host unreachable.
```

Voice, CCTV and Management restrictions were tested using existing SVI addresses rather than dedicated endpoint devices.

The tests demonstrated that the Guest ACL was restricting the specified internal destinations while preserving access to the Guest gateway.

**CORE-L3-SW1 Guest Isolation Verification Status:** Passed for the eight tested destination VLANs.

### 8.7 ACL Match Counter Verification

The following Cisco IOS command was used to inspect the Guest ACL:

```cisco
show ip access-lists GUEST_ISOLATION
```

CORE-L3-SW1 displayed match counters for the tested deny rules.

| ACL Rule | Observed Matches |
|---|---|
| Guest → Finance | 4 |
| Guest → Servers | 4 |
| Final permit rule | 4 |

The match counters confirmed that packets were matching the configured ACL statements.

The final permit-rule counter did not independently identify which permitted traffic generated those matches.

### 8.8 Guest Isolation Verification — CORE-L3-SW2

To verify the alternate routing path, the Guest workstation default gateway was temporarily changed:

**Original Gateway:** 192.168.80.2

**Temporary Gateway:** 192.168.80.3

The temporary gateway directed routed Guest traffic through CORE-L3-SW2.

The following tests were performed:

| Test | Result |
|---|---|
| Guest → CORE-L3-SW2 Gateway | Allowed — 4/4 replies |
| Guest → Finance PC | Blocked — 0/4 replies |

The blocked Finance test returned:

```text
Reply from 192.168.80.3: Destination host unreachable.
```

The Guest ACL on CORE-L3-SW2 also displayed four matches against its Finance deny statement.

These results confirmed that the second core switch independently enforced the Guest-to-Finance restriction.

After testing, the Guest workstation default gateway was restored to 192.168.80.2.

A final verification confirmed:

- Guest → 192.168.80.2: Allowed.
- Guest → 192.168.30.10: Blocked.

**CORE-L3-SW2 Guest-to-Finance Verification Status:** Passed.

### 8.9 ACL Verification Commands

The following commands were used to inspect ACL configuration and enforcement:

```cisco
show ip access-lists SALES_BLOCK_FINANCE
show ip access-lists GUEST_ISOLATION
show running-config
show ip interface brief
```

Verification included:

1. Confirming that the ACL statements and wildcard masks were correct.
2. Checking that the ACLs existed on both core switches.
3. Confirming that the ACLs were applied inbound on the correct SVIs.
4. Comparing connectivity before and after ACL implementation.
5. Verifying that intended permitted communication remained functional.
6. Checking ACL match counters where supported.
7. Testing Guest-to-Finance restrictions through both core switches.
8. Restoring the Guest workstation to its original network configuration.

### 8.10 Rollback Procedure

If an ACL causes unexpected disruption, the interface binding can be removed without deleting the ACL definition.

Example rollback for Sales restrictions:

```cisco
configure terminal
interface Vlan10
 no ip access-group SALES_BLOCK_FINANCE in
end
```

Example rollback for Guest isolation:

```cisco
configure terminal
interface Vlan80
 no ip access-group GUEST_ISOLATION in
end
```

These commands must be performed on the affected core switch, with changes coordinated across both switches if the policy is being rolled back completely.

After rollback, the original connectivity tests should be repeated to confirm restoration.

The pre-ACL Packet Tracer checkpoint was preserved as:

`Northstar-Project01-Pre-ACL-v09.pkt`

The validated ACL configuration was saved as:

`Northstar-Project01-ACL-Validated-v10.pkt`

### 8.11 Security Limitations and Outstanding Work

The implemented policies were verified against the planned test cases, but further security work remains.

Outstanding items include:

- Extend Finance protection to other unauthorised source VLANs where required.
- Test Guest restrictions through CORE-L3-SW2 for destinations beyond Finance.
- Verify ACL behaviour during HSRP failover.
- Test additional application protocols rather than ICMP alone.
- Validate security controls for newly introduced internal subnets.
- Implement appropriate firewall, internet access and network management restrictions.
- Review traffic initiated from internal networks toward Guest devices, because the current inbound Guest ACL primarily filters traffic entering the routing interface from VLAN 80.
- Review VLAN-level and switchport security controls for a production design.

The successful ping and ACL-counter tests demonstrate the intended behaviour for the tested traffic. They do not constitute a complete network security audit.

### 8.12 Implementation Status

**Sales-to-Finance Restriction:** Implemented and tested on the current routing path.

**Authorised IT-to-Finance Access:** Verified.

**Guest Isolation:** Implemented on both core switches.

**Guest-to-Finance Redundant Path Verification:** Passed on both core switches.

**Additional Security Validation:** Pending.

**Environment:** Cisco Packet Tracer simulation.


## 9. Implementation Issues, Troubleshooting and Lessons Learned

### 9.1 Troubleshooting Objective

Document configuration issues, unexpected behaviour and corrective actions encountered during the simulated network implementation.

The troubleshooting approach involved:

1. Identifying the observed symptom.
2. Checking the relevant device configuration and operational status.
3. Comparing the results against the intended network design.
4. Making a controlled correction.
5. Repeating verification commands or connectivity tests.
6. Recording the outcome and any remaining limitations.

This approach was used to distinguish configuration errors from expected networking behaviour.

### 9.2 Implementation Issues Summary

| Issue ID | Observed Issue | Corrective Action | Result |
|---|---|---|---|
| T01 | Unexpected STP root bridge election | Configured explicit core-switch bridge priorities | Root bridge selection corrected for the VLAN tested |
| T02 | Some initial ping attempts experienced packet loss | Repeated connectivity tests after the network had settled | Subsequent tests succeeded |
| T03 | Cisco IOS commands were rejected because of syntax or CLI mode | Corrected command syntax and used appropriate configuration or verification mode | Commands executed successfully |
| T04 | HSRP initially displayed unexpected Active/Standby roles | Reviewed HSRP priorities and configured preferred gateway roles | Intended roles verified |
| T05 | Initial test-server address conflicted with the reserved gateway addressing plan | Changed the server address to 192.168.50.10 | Server connectivity verified |
| T06 | Interface descriptions required correction | Updated the affected switchport descriptions | Port documentation corrected |
| T07 | Inter-VLAN communication was permitted before ACL implementation | Applied extended ACLs and performed before-and-after testing | Intended traffic restrictions verified |

### 9.3 Issue T01 — Unexpected STP Root Bridge Election

**Observed Symptom:**

During the initial STP configuration, root bridge selection did not consistently follow the intended core-switch hierarchy.

An access switch could become the root bridge when the participating switches retained their default bridge priorities.

**Investigation:**

STP election information was examined using:

```cisco
show spanning-tree vlan 1
```

The output was used to compare the Root ID and Bridge ID information.

**Corrective Action:**

Explicit STP priorities were configured for VLAN 1:

```cisco
! CORE-L3-SW1
spanning-tree vlan 1 priority 4096

! CORE-L3-SW2
spanning-tree vlan 1 priority 8192
```

**Verification:**

Subsequent STP checks confirmed the intended core-switch root hierarchy for VLAN 1.

Later testing also identified different root bridges and forwarding paths for VLANs 10 and 20.

**Lesson Learned:**

STP root bridge selection should be intentional. Relying only on default priorities can produce unexpected root bridge elections because MAC addresses influence the election when priorities are equal.

### 9.4 Issue T02 — Initial Ping Packet Loss

**Observed Symptom:**

Some connectivity tests initially returned partial packet loss, including results of 50% packet loss.

When the same tests were repeated, communication succeeded with 0% packet loss.

**Investigation:**

The following areas were considered during connectivity verification:

- Endpoint IP addresses and subnet masks.
- Default gateway configuration.
- Access-port VLAN assignments.
- Trunk connectivity.
- SVI operational status.
- Layer 3 routing information.

Initial address resolution or network convergence may have contributed to transient packet loss, but an exact root cause was not conclusively established.

**Corrective Action:**

Connectivity tests were repeated after allowing the simulated network to settle.

**Verification:**

Subsequent tests showed successful communication between the tested endpoints.

**Lesson Learned:**

A single failed ping is insufficient to determine that a network path is permanently unavailable.

Engineers should repeat tests, inspect relevant device information and distinguish transient behaviour from persistent faults.

### 9.5 Issue T03 — Cisco IOS Command Syntax and CLI Modes

**Observed Symptom:**

Some Cisco IOS commands were rejected with messages such as:

```text
% Invalid input detected at '^' marker.
```

Other incomplete commands were rejected because required parameters were missing.

**Investigation:**

The command syntax and current Cisco IOS configuration mode were reviewed.

During the lab, verification commands were sometimes issued from interface or global configuration mode.

**Corrective Action:**

Commands were corrected and executed in the appropriate mode.

For example, an operational verification command can be executed from configuration mode by using the `do` keyword:

```cisco
do show ip route
do show standby brief
do show ip access-lists GUEST_ISOLATION
```

Alternatively, configuration mode can be exited before running operational commands.

**Verification:**

The corrected commands returned routing, HSRP and ACL information successfully.

**Lesson Learned:**

Understanding Cisco IOS command modes and using command-line help are essential troubleshooting skills.

An invalid command does not necessarily indicate a device fault; the issue may be incorrect syntax, the current command mode or an unsupported feature.

### 9.6 Issue T04 — HSRP Active/Standby Role Selection

**Observed Symptom:**

During initial HSRP configuration, the active gateway roles did not immediately match the intended design.

CORE-L3-SW2 temporarily appeared as Active for both configured HSRP groups during setup.

**Investigation:**

The following command was used to inspect group states and priorities:

```cisco
show standby brief
```

The preferred active gateway design was:

- VLAN 10: CORE-L3-SW1
- VLAN 20: CORE-L3-SW2

**Corrective Action:**

HSRP priorities and preemption were configured so that each preferred core switch had a higher priority for its assigned VLAN.

**Verification:**

The final HSRP status showed:

| VLAN | Active Gateway | Standby Gateway |
|---|---|---|
| 10 — SALES | CORE-L3-SW1 | CORE-L3-SW2 |
| 20 — OPERATIONS | CORE-L3-SW2 | CORE-L3-SW1 |

Controlled SVI shutdown and restoration tests demonstrated successful HSRP role changes.

**Lesson Learned:**

HSRP configuration requires more than assigning a virtual IP address.

Gateway priority, preemption and operational state must be verified to ensure traffic follows the intended active/standby design.

### 9.7 Issue T05 — Test Server IP Address Correction

**Observed Issue:**

GF-TEST-SERVER1 was initially configured with the address `192.168.50.1`.

However, the addressing plan reserved the `.1` address in each VLAN subnet for a future HSRP virtual gateway.

**Corrective Action:**

The server IP configuration was updated to:

| Parameter | Corrected Value |
|---|---|
| IP Address | 192.168.50.10 |
| Subnet Mask | 255.255.255.224 |
| Default Gateway | 192.168.50.2 |
| VLAN | 50 — SERVERS |

**Verification:**

The server successfully pinged:

- Its default gateway, 192.168.50.2.
- The IT workstation, 192.168.40.10.

Both tests completed with 0% packet loss.

**Lesson Learned:**

IP addressing plans must be checked before assigning addresses to endpoints.

Reserving addresses for virtual gateways and infrastructure devices helps prevent future conflicts.

### 9.8 Issue T06 — Interface Description Corrections

**Observed Issue:**

Some switchport descriptions needed to be updated to match the intended connected devices.

For example, the Operations access port on FF-ACC-SW2 required a description correction.

**Corrective Action:**

The affected interface descriptions were updated to reflect their intended endpoint roles.

**Verification:**

The following command was used to review switchport information:

```cisco
show interfaces status
```

**Lesson Learned:**

Accurate interface descriptions reduce troubleshooting time and make it easier to identify physical connections, particularly when supporting a network remotely.

### 9.9 Issue T07 — Inter-VLAN Access Before Security Enforcement

**Observed Issue:**

Before ACL implementation, Sales and Guest workstations could communicate with internal Finance endpoints through the Layer 3 infrastructure.

**Investigation:**

Successful ping tests demonstrated that VLAN segmentation and inter-VLAN routing were operational.

However, VLAN separation did not prevent communication once routing was enabled.

**Corrective Action:**

Extended IP ACLs were configured on both core switches:

- `SALES_BLOCK_FINANCE`
- `GUEST_ISOLATION`

The ACLs were attached inbound to their respective source VLAN SVIs.

**Verification:**

After implementation:

- Sales-to-Finance traffic was blocked.
- Guest-to-Finance traffic was blocked through both core switches.
- Guest access to other tested internal VLAN destinations was blocked.
- IT-to-Finance traffic remained permitted.
- Sales-to-Server traffic remained permitted.
- Guest gateway connectivity remained operational.

ACL match counters provided additional confirmation that selected deny statements were matching packets.

**Lesson Learned:**

VLAN segmentation and network access control serve different purposes.

VLANs create separate Layer 2 broadcast domains, while Layer 3 ACLs restrict selected routed traffic.

Security changes should be validated using both negative tests (traffic that must fail) and positive tests (traffic that must succeed).

### 9.10 Key Technical Lessons Learned

The simulated implementation reinforced several network engineering principles:

1. **Design before configuration:** Document VLANs, subnet allocations, physical interfaces and security requirements before implementing changes.
2. **Verify each implementation phase:** Use operational commands and endpoint tests instead of assuming a configuration is functional.
3. **Configure predictable STP behaviour:** Explicit root bridge priorities reduce unintended root elections.
4. **Understand redundancy mechanisms:** STP manages Layer 2 paths, while HSRP provides first-hop gateway redundancy.
5. **Distinguish network layers:** Same-VLAN traffic is switched at Layer 2; inter-VLAN traffic is routed at Layer 3.
6. **Apply least-privilege principles:** Permit authorised traffic and deny explicitly prohibited communication based on business requirements.
7. **Test security from both directions where applicable:** A successful test on one routing path does not automatically prove that every alternative path is secured.
8. **Preserve rollback checkpoints:** Save known-working configurations before applying changes that may affect connectivity.
9. **Document limitations:** Clearly distinguish verified functionality from planned features and outstanding testing.
10. **Use evidence:** CLI outputs, ping results and configuration checkpoints make troubleshooting conclusions more defensible.

### 9.11 Remaining Technical Risks

The following items still require attention before the simulated design can be considered fully validated:

- Complete gateway redundancy for the remaining VLANs where required.
- Test physical uplink and core-switch failures.
- Measure packet loss and recovery time during failover.
- Verify ACL behaviour during failure and recovery scenarios.
- Complete Voice and CCTV endpoint verification.
- Review network management security and unused-port controls.
- Implement and test upstream firewall and internet connectivity.
- Confirm the final STP root bridges and forwarding roles across all required VLANs.

These items will be addressed in subsequent implementation and resilience testing phases.

**Troubleshooting Documentation Status:** Completed for the issues recorded during the current implementation.

**Outstanding Remediation and Validation:** Pending for the items identified above.

**Environment:** Cisco Packet Tracer simulation.


## 10. Implementation Summary, Outstanding Work and Evidence

### 10.1 Implementation Summary

The Northstar Business Solutions network was implemented in Cisco Packet Tracer to simulate a segmented small-business network with redundant switching infrastructure.

The implementation demonstrated VLAN configuration, Layer 2 trunking, inter-VLAN routing, first-hop gateway redundancy, network access control and structured troubleshooting.

Verification was performed using Cisco IOS operational commands and endpoint connectivity tests.

### 10.2 Implementation Status Matrix

| Component | Implementation Status | Verification Status |
|---|---|---|
| Physical switch topology | Implemented | 11 inter-switch links mapped using CDP |
| VLANs 10–90 | Implemented | VLAN assignments and connected endpoints checked |
| 802.1Q trunks | Implemented | Basic trunk operation verified |
| STP | Configured | Detailed results recorded for VLANs 10 and 20; remaining checks pending |
| Access ports | Implemented | Connected endpoints and unused ports checked |
| Inter-VLAN routing | Implemented | Tested endpoint connectivity passed |
| SVI addressing | Implemented | Nine SVIs configured on each core switch |
| HSRP — VLAN 10 | Implemented | Control-plane failover passed |
| HSRP — VLAN 20 | Implemented | Control-plane failover passed |
| HSRP — Remaining VLANs | Pending | Not yet tested |
| Sales-to-Finance ACL | Implemented | Blocked traffic verified |
| Guest isolation ACL | Implemented | Eight internal VLAN destinations tested via SW1; Finance tested via SW2 |
| Voice endpoint | Pending | Not yet connected and tested |
| CCTV endpoint | Pending | Not yet connected and tested |
| Physical uplink failover | Pending | Controlled failure tests not yet performed |
| Internet and firewall connectivity | Pending | Not yet implemented |

The status matrix reflects the current simulated implementation rather than a production network.

### 10.3 Verified Functional Requirements

The following results were demonstrated during the lab:

1. Connected workstations were assigned to their intended VLANs.
2. Operations workstations communicated across floors within VLAN 20.
3. Layer 3 switching enabled inter-VLAN communication before ACL enforcement.
4. HSRP active and standby roles were established for VLANs 10 and 20.
5. Both tested HSRP groups changed active gateway roles during SVI shutdown tests.
6. Sales-to-Finance traffic was blocked after ACL implementation.
7. IT-to-Finance traffic remained permitted.
8. Sales-to-Server traffic remained permitted.
9. Guest traffic was blocked from all eight tested internal destination VLANs through CORE-L3-SW1.
10. Guest-to-Finance traffic was independently blocked through CORE-L3-SW2.
11. Guest connectivity to its own configured gateway remained operational.
12. Selected ACL deny rules displayed packet match counters.

These results confirm the observed behaviour of the tested configurations. They do not prove every possible traffic path or failure scenario.

### 10.4 Outstanding Implementation and Testing

The following activities remain before the complete design can be considered validated:

- Configure first-hop gateway redundancy for the remaining VLANs where required.
- Verify STP root bridges and port roles for all planned VLANs.
- Perform controlled physical uplink failure tests.
- Measure packet loss and approximate recovery time during failover.
- Verify that ACL restrictions remain effective during gateway failover.
- Connect and test Voice and CCTV endpoints.
- Review management access restrictions and additional switchport security.
- Implement firewall connectivity and internet routing.
- Test permitted application services beyond basic ICMP connectivity.
- Review the implemented network against the original business requirements.

Outstanding tasks should be completed and documented as separate, controlled changes.

### 10.5 Configuration Checkpoints

The following Packet Tracer checkpoints were retained during the implementation.

| Checkpoint | Filename | Purpose |
|---|---|---|
| Pre-ACL | Northstar-Project01-Pre-ACL-v09.pkt | Connectivity baseline before security policy enforcement |
| Post-ACL | Northstar-Project01-ACL-Validated-v10.pkt | Validated Sales and Guest ACL configuration |

The checkpoints provide reference points for troubleshooting, comparison and rollback.

They represent saved simulation files and are not evidence of production deployment.

### 10.6 Evidence Collection Checklist

The following evidence should be organised and added to the project repository.

#### Network Design and Switching

- [ ] Final Packet Tracer topology screenshot.
- [ ] VLAN table verification from the core switches.
- [ ] VLAN and access-port assignments from representative access switches.
- [ ] Trunk operation and allowed VLAN verification.
- [ ] CDP neighbour output confirming inter-switch links.
- [ ] STP root bridge and port-role verification.

#### Routing and Redundancy

- [ ] SVI operational status on CORE-L3-SW1.
- [ ] SVI operational status on CORE-L3-SW2.
- [ ] Connected and local routing-table entries.
- [ ] HSRP active and standby states for VLANs 10 and 20.
- [ ] HSRP failover and restoration results.
- [ ] Successful same-VLAN and inter-VLAN connectivity tests.

#### Network Security

- [ ] Sales-to-Finance connectivity before ACL implementation.
- [ ] Sales-to-Finance blocked connectivity after ACL implementation.
- [ ] Successful IT-to-Finance connectivity after ACL implementation.
- [ ] Successful Sales-to-Server connectivity after ACL implementation.
- [ ] Guest-to-Finance blocked connectivity through CORE-L3-SW1.
- [ ] Guest-to-Server blocked connectivity.
- [ ] Guest isolation tests for the remaining defined internal VLANs.
- [ ] Guest-to-Finance blocked connectivity through CORE-L3-SW2.
- [ ] ACL match counter verification from both core switches.
- [ ] Final Guest gateway connectivity verification.

Some evidence was captured during the implementation walkthrough. The checklist tracks its organisation and inclusion in the GitHub repository, not whether every screenshot has already been uploaded.

### 10.7 Final Assessment

The implemented lab demonstrates the integration of Layer 2 switching, Layer 3 routing, first-hop redundancy and IP access controls in a simulated multi-VLAN business network.

The strongest verified outcomes include:

- Correct departmental VLAN segmentation.
- Successful routing between permitted internal networks.
- HSRP role failover for two configured VLANs.
- Selective blocking of Sales-to-Finance traffic.
- Guest isolation across tested internal destinations.
- Consistent Guest-to-Finance restrictions on both core switches.
- Preservation of permitted communication following ACL implementation.

The implementation also identified additional work required to validate full network resilience, gateway redundancy, endpoint coverage and perimeter security.

**Current Project Status:** Core network functionality implemented and tested for the documented scenarios.

**Security Validation Status:** Passed for the recorded ACL tests.

**Full Resilience Validation Status:** Incomplete — additional testing required.

**Deployment Environment:** Cisco Packet Tracer simulation.

**Production Deployment Status:** Not deployed.
