
# Project 01 — Method of Procedure (MOP)

**Client:** Northstar Business Solutions (Fictional)  
**Project:** Small Business Network Design, VLAN Segmentation & Inter-VLAN Routing  
**Environment:** Cisco Packet Tracer  
**Document Version:** 1.0  
**Document Status:** Draft — Pending Review and Approval  
**Change Type:** Planned Network Configuration Implementation  

---

## 1. Purpose of the MOP

This Method of Procedure (MOP) describes the planned steps for implementing, verifying and, if necessary, rolling back network configuration changes in the Northstar Business Solutions simulated environment.

The purpose of this document is to ensure that network changes are performed in a controlled, documented and repeatable manner.

The MOP will define:

- Change objectives and scope.
- Prerequisites and dependencies.
- Potential risks and expected impact.
- Pre-implementation verification checks.
- Reviewed configuration commands.
- Implementation sequence.
- Post-implementation verification.
- Rollback procedures.
- Success criteria and evidence collection.

## 2. Change Objective

The objective of this planned change is to implement a secure, segmented and resilient internal network supporting Northstar's business requirements.

The implementation will build on the existing physical switching topology and preliminary network design.

The intended outcome is to provide:

1. Departmental segmentation using VLANs.
2. Controlled communication between VLANs.
3. Appropriate IP addressing and default gateways.
4. Redundant Layer 3 gateway functionality, subject to simulator support.
5. Controlled Layer 2 redundancy using Spanning Tree Protocol.
6. Security restrictions protecting sensitive network segments.
7. Verified network connectivity and documented test results.

## 3. Change Scope

### 3.1 In Scope

The planned implementation includes:

- Creating the nine proposed VLANs.
- Configuring access and trunk ports.
- Configuring Layer 3 Switch Virtual Interfaces (SVIs).
- Enabling IP routing where required.
- Configuring and verifying STP for the required VLANs.
- Evaluating and implementing VRRP or a supported equivalent.
- Configuring appropriate inter-VLAN access controls.
- Testing connectivity, segmentation and failover.
- Recording results and troubleshooting findings.

### 3.2 Out of Scope

The following activities are not included in this initial configuration change:

- Production network deployment.
- Purchasing physical network equipment.
- Installing physical fibre-optic cabling.
- Configuring production ISP connections.
- Deploying production firewall high availability.
- Migrating live business applications.

These activities require separate planning and validation.

### 3.3 Previously Completed Lab Activities

The following work was completed before this MOP was prepared:

- Deployment and naming of seven simulated switches.
- Installation of eleven simulated network connections.
- Verification of physical neighbours using CDP.
- Configuration of CORE-L3-SW1 as the preferred STP root for VLAN 1.
- Configuration of CORE-L3-SW2 as the secondary STP root for VLAN 1.

These activities form the existing lab baseline and will not be represented as future changes.


## 4. Prerequisites and Pre-Implementation Checks

### 4.1 Prerequisites

Before beginning the planned network configuration, the following requirements must be satisfied:

- [ ] Review the Business Requirements Document.
- [ ] Review the preliminary Network Design Document.
- [ ] Verify the Physical Port Mapping Document.
- [ ] Confirm access to all seven Cisco switches.
- [ ] Confirm that the Packet Tracer topology has been saved.
- [ ] Create a backup copy of the existing Packet Tracer project.
- [ ] Review the proposed VLAN IDs and IP addressing plan.
- [ ] Confirm simulator support for the proposed routing and redundancy features.
- [ ] Prepare and review configuration commands before execution.
- [ ] Define success criteria and rollback procedures.

**Baseline File:** Northstar-Project01-Topology-v03.pkt

The existing topology will serve as the reference point for the planned configuration changes.

### 4.2 Pre-Implementation Verification Commands

The following commands will be used to establish the network baseline.

#### Layer 3 Core Switches

Run these commands on CORE-L3-SW1 and CORE-L3-SW2:

```text
show version
show running-config
show ip interface brief
show interfaces status
show vlan brief
show interfaces trunk
show spanning-tree vlan 1
show cdp neighbors
```

These commands will help verify:

- Device software and hardware information.
- Existing running configurations.
- Interface operational status.
- Existing VLAN configuration.
- Current trunk configuration.
- STP root bridge and interface states.
- Directly connected Cisco neighbours.

#### Layer 2 Access Switches

Run the following commands on the five access switches:

```text
show running-config
show interfaces status
show vlan brief
show interfaces trunk
show spanning-tree vlan 1
show cdp neighbors
```

These checks will establish the current access-layer configuration before VLAN and trunk implementation.

### 4.3 Existing Verified Baseline

The following results have already been observed in the simulation:

| Verification Item | Observed Result |
|---|---|
| Deployed Switches | 7 |
| Physical Connections | 11 |
| CDP Neighbours on CORE-L3-SW1 | 6 |
| CDP Neighbours on CORE-L3-SW2 | 6 |
| Preferred STP Root for VLAN 1 | CORE-L3-SW1 |
| Secondary STP Root for VLAN 1 | CORE-L3-SW2 |
| Proposed Departmental VLAN Configuration | Not yet implemented |
| Inter-VLAN Routing | Not yet configured |
| VRRP | Not yet configured |

The completed CDP and STP checks will be supplemented with fresh pre-implementation checks before configuration begins.

### 4.4 Go / No-Go Criteria

Implementation must not begin until:

1. The current Packet Tracer configuration has been backed up.
2. Existing interface and device connectivity has been verified.
3. Planned commands have been reviewed for compatibility.
4. Expected risks and impacts have been documented.
5. A workable rollback procedure has been defined.
6. The simulated change has been approved for implementation.

**Status:** Pre-implementation checklist prepared. Outstanding verification and approval steps must be completed before execution.


## 5. Risk Assessment and Expected Impact

### 5.1 Risk Assessment

The following risks have been identified for the planned network configuration changes.

| Risk ID | Potential Risk | Impact | Mitigation |
|---|---|---|---|
| R01 | Incorrect VLAN configuration | Devices may be unable to communicate within their intended VLAN | Verify VLAN IDs and assignments before and after configuration |
| R02 | Incorrect trunk configuration | VLAN traffic may fail between switches | Review allowed VLANs and verify trunk status |
| R03 | Incorrect STP configuration | Switching loops, blocked paths or network disruption | Verify STP root priorities and interface states |
| R04 | Incorrect SVI or gateway configuration | Devices may lose inter-VLAN connectivity | Validate IP addresses, subnet masks and gateway assignments |
| R05 | Incorrect ACL configuration | Unauthorised access or legitimate traffic being blocked | Review ACL entries and test permitted and denied traffic |
| R06 | Gateway redundancy misconfiguration | Default gateway failover may not operate as expected | Verify simulator support and test failover |
| R07 | Unexpected configuration behaviour | Temporary loss of simulated network connectivity | Apply changes incrementally and maintain rollback checkpoints |

These risks are preliminary and will be reviewed before implementation.

### 5.2 Expected Impact

The planned configuration changes may temporarily affect connectivity within the Cisco Packet Tracer simulation.

Potential impacts include:

- Temporary interruption of traffic during STP reconvergence.
- Loss of connectivity if VLANs are incorrectly assigned.
- Inability to communicate across switches if trunks are misconfigured.
- Inter-VLAN connectivity failures caused by incorrect gateway configuration.
- Blocked or unauthorised traffic resulting from incorrect ACL configuration.
- Temporary connectivity disruption during gateway failover testing.

No production systems or real users will be affected because this is an isolated simulation.

### 5.3 Risk Mitigation Strategy

To reduce implementation risks:

1. Review configuration commands before execution.
2. Back up the existing Packet Tracer topology.
3. Apply changes in a controlled sequence.
4. Verify each configuration stage before proceeding.
5. Avoid making multiple unverified changes simultaneously.
6. Record unexpected behaviour and troubleshooting findings.
7. Maintain a tested rollback procedure.
8. Compare post-implementation results with the baseline.

### 5.4 Stop and Rollback Conditions

Implementation must be paused if:

- Unexpected switching loops or persistent instability occur.
- Critical connectivity cannot be restored.
- Required VLANs become unexpectedly unreachable.
- Security controls permit unauthorised access to restricted networks.
- Configuration behaviour differs significantly from the approved design.

If an issue cannot be resolved safely, the affected changes must be reversed or the saved Packet Tracer baseline restored.

**Status:** Preliminary risk assessment prepared. Risks and rollback procedures require final review before implementation.


## 6. Implementation Plan and Configuration Sequence

### 6.1 Implementation Approach

The network configuration will be implemented in controlled stages.

Each stage must be verified before proceeding to the next.

Configuration commands will be prepared and reviewed before execution.

**Implementation Status:** Planned — Not Started

### 6.2 Planned Implementation Sequence

| Phase | Activity | Verification |
|---|---|---|
| 1 | Complete pre-implementation checks and backups | Confirm baseline, connectivity and rollback readiness |
| 2 | Create the nine proposed VLANs | Verify using show vlan brief |
| 3 | Configure STP priorities for the required VLANs | Verify root bridge and port roles |
| 4 | Configure trunk links between switches | Verify using show interfaces trunk |
| 5 | Configure access ports for endpoint devices | Verify VLAN assignments and port status |
| 6 | Configure Layer 3 SVIs and enable IP routing | Verify SVI status and routing tables |
| 7 | Implement gateway redundancy if supported | Verify gateway roles and failover |
| 8 | Configure inter-VLAN security policies | Test permitted and denied traffic |
| 9 | Perform end-to-end connectivity and failover tests | Record test results and observed behaviour |
| 10 | Review configuration and save verified changes | Capture final configuration and evidence |

### 6.3 Implementation Controls

The following controls will apply throughout implementation:

- Use the verified Physical Port Mapping Document.
- Confirm interface numbers before applying commands.
- Apply and verify changes in small stages.
- Record unexpected command errors and behaviour.
- Verify STP stability after trunk configuration.
- Confirm simulator compatibility before using redundancy features.
- Do not proceed when a critical verification test fails.
- Maintain working rollback checkpoints.

### 6.4 Configuration Review

Detailed Cisco IOS commands will be documented separately within this MOP before execution.

The commands must specify:

- Target device.
- Target interface or VLAN.
- Intended configuration.
- Expected outcome.
- Verification command.
- Rollback command or recovery procedure.

### 6.5 Implementation Hold Points

Implementation must pause after major configuration stages to verify the results.

In particular:

1. Confirm VLAN creation before trunk configuration.
2. Confirm STP stability and trunk operation before enabling inter-VLAN routing.
3. Confirm routing functionality before applying restrictive ACLs.
4. Confirm redundancy behaviour through controlled failure tests.
5. Confirm all required connectivity and security tests before declaring completion.

**Status:** Preliminary implementation sequence prepared. Detailed configurations, validation results and approval remain outstanding.


## 7. Detailed Configuration Procedures

### 7.1 VLAN Creation

**Implementation Phase:** 2  
**Target Devices:** All seven Cisco switches  
**Change Status:** Planned — Not Executed

#### Objective

Create the nine VLANs defined in the Network Design Document.

These VLANs will provide the logical segmentation required for Northstar's departments and network services.

#### Pre-Implementation Check

Before creating VLANs, verify the existing VLAN configuration.

**Command:**

```text
show vlan brief
```

Record the existing VLANs and confirm that the proposed VLAN IDs do not conflict with existing configurations.

#### Proposed Configuration Commands

The following commands will be reviewed and applied to each switch, subject to successful pre-implementation checks.

```text
enable
configure terminal

vlan 10
 name SALES

vlan 20
 name OPERATIONS

vlan 30
 name FINANCE

vlan 40
 name IT

vlan 50
 name SERVERS

vlan 60
 name VOICE

vlan 70
 name CCTV

vlan 80
 name GUEST

vlan 90
 name MANAGEMENT

end
```

#### Post-Configuration Verification

Run:

```text
show vlan brief
```

**Expected Result:**

- VLAN 10 — SALES exists.
- VLAN 20 — OPERATIONS exists.
- VLAN 30 — FINANCE exists.
- VLAN 40 — IT exists.
- VLAN 50 — SERVERS exists.
- VLAN 60 — VOICE exists.
- VLAN 70 — CCTV exists.
- VLAN 80 — GUEST exists.
- VLAN 90 — MANAGEMENT exists.

VLAN 1 remains present as a default VLAN.

No endpoint access ports or uplink trunks will be changed during this phase.

#### Rollback Procedure

If VLAN creation fails or produces unexpected behaviour:

1. Stop the implementation.
2. Identify and document the issue.
3. Review the running configuration and VLAN database.
4. Remove only newly created VLANs when safe and when no interfaces or services depend on them.
5. If necessary, restore the saved Packet Tracer baseline.
6. Verify that the original configuration has been restored.

#### Success Criteria

This phase is successful when all nine required VLANs are visible on the intended switches, no unintended configuration changes have occurred, and the switching topology remains stable.

**Phase Status:** Pending command review and implementation approval.


### 7.2 Spanning Tree Protocol (STP) Configuration

**Implementation Phase:** 3  
**Target Devices:** CORE-L3-SW1 and CORE-L3-SW2  
**Change Status:** Planned — Not Executed



#### Objective

Configure a predictable STP root bridge hierarchy for the nine proposed VLANs.

The preferred STP root bridge will be distributed between CORE-L3-SW1 and CORE-L3-SW2 to align with the planned HSRP active gateway roles.

- CORE-L3-SW1 will be the preferred root for VLANs 10, 30, 50, 70 and 90.
- CORE-L3-SW2 will be the preferred root for VLANs 20, 40, 60 and 80.
- Each core switch will serve as the secondary root for VLANs where the other switch is preferred.

The existing VLAN 1 STP configuration will be preserved.

#### Pre-Implementation Checks

Before applying the new STP priorities:

1. Confirm that the required VLANs have been created.
2. Verify the existing STP configuration.
3. Confirm the intended root bridge priorities.
4. Verify the baseline configuration has been backed up.

**Verification Commands:**

```text
show spanning-tree vlan 1
show running-config
show vlan brief
```

#### Proposed Configuration — CORE-L3-SW1

Configure the preferred root bridge priority.

```text
enable
configure terminal


spanning-tree vlan 10 priority 4096
spanning-tree vlan 20 priority 8192
spanning-tree vlan 30 priority 4096
spanning-tree vlan 40 priority 8192
spanning-tree vlan 50 priority 4096
spanning-tree vlan 60 priority 8192
spanning-tree vlan 70 priority 4096
spanning-tree vlan 80 priority 8192
spanning-tree vlan 90 priority 4096


end
```

#### Proposed Configuration — CORE-L3-SW2

Configure the STP root bridge priorities to align with the planned HSRP active gateway roles

```text
enable
configure terminal


spanning-tree vlan 10 priority 8192
spanning-tree vlan 20 priority 4096
spanning-tree vlan 30 priority 8192
spanning-tree vlan 40 priority 4096
spanning-tree vlan 50 priority 8192
spanning-tree vlan 60 priority 4096
spanning-tree vlan 70 priority 8192
spanning-tree vlan 80 priority 4096
spanning-tree vlan 90 priority 8192


end
```

#### Post-Configuration Verification

Verify that the intended priorities have been configured on both core switches.

Example commands:

```text
show running-config
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
```

Repeat the STP checks for the remaining VLANs.

**Expected Results:**

- CORE-L3-SW1 has an STP priority of 4096 for VLANs 10, 30, 50, 70 and 90.
- CORE-L3-SW1 has an STP priority of 8192 for VLANs 20, 40, 60 and 80.
- CORE-L3-SW2 has an STP priority of 4096 for VLANs 20, 40, 60 and 80.
- CORE-L3-SW2 has an STP priority of 8192 for VLANs 10, 30, 50, 70 and 90.
- Access switches retain their default STP priorities unless an approved change is required.
- CORE-L3-SW1 becomes the preferred STP root for VLANs 10, 30, 50, 70 and 90.
- CORE-L3-SW2 becomes the preferred STP root for VLANs 20, 40, 60 and 80.
- Each non-root core switch is positioned to take over root responsibilities if the preferred root fails.
- STP maintains a loop-free Layer 2 topology across redundant connections.
- The existing VLAN 1 STP configuration remains unchanged.

The actual STP root bridge election and forwarding paths must be verified after the required trunk links are operational.

The planned STP root allocation is designed to align with the HSRP active gateway roles once HSRP is implemented.


**Verification Note:**

Before trunk links are configured to carry the new VLANs, the complete network-wide STP topology may not yet be established.

Root bridge election, forwarding paths and blocked ports must be verified again after trunk configuration.

#### Rollback Procedure

If unexpected behaviour occurs:

1. Stop the implementation.
2. Record the affected VLANs and interfaces.
3. Compare the configuration with the saved baseline.
4. Restore the previous STP priority settings where necessary.
5. Verify STP stability and root bridge election.
6. Restore the saved Packet Tracer baseline if recovery is unsuccessful.

For newly created VLANs whose original priority was the default, the following command illustrates restoring the default priority:

```text
configure terminal
spanning-tree vlan 10 priority 32768
end
```

Repeat only for affected VLANs after confirming their original settings.

Do not disable STP as a general rollback method.

#### Success Criteria

The phase is successful when:

- The planned STP priorities are configured correctly.
- The primary and secondary root hierarchy is verified after trunk configuration.
- No unexpected switching loops occur.
- Redundant paths behave as intended.
- VLAN 1 retains its previously verified STP configuration.

**Phase Status:** Planned — Pending implementation and verification.


### 7.3 VLAN Trunk Configuration

**Implementation Phase:** 4  
**Target Devices:** All seven Cisco switches  
**Change Status:** Planned — Not Executed

#### Objective

Configure IEEE 802.1Q trunk links between the Layer 3 core switches and Layer 2 access switches.

The trunk links will carry the required VLAN traffic across the Northstar network while maintaining logical VLAN separation.

#### Verified Trunk Interface Assignments

| Device | Interfaces |
|---|---|
| CORE-L3-SW1 | Gi1/0/1 through Gi1/0/5, Gi1/0/23 |
| CORE-L3-SW2 | Gi1/0/1 through Gi1/0/5, Gi1/0/23 |
| GF-ACC-SW1 | Gi0/1, Gi0/2 |
| GF-ACC-SW2 | Gi0/1, Gi0/2 |
| GF-ACC-SW3 | Gi0/1, Gi0/2 |
| FF-ACC-SW1 | Gi0/1, Gi0/2 |
| FF-ACC-SW2 | Gi0/1, Gi0/2 |

All interface assignments were verified through Cisco Discovery Protocol (CDP).

#### Preliminary Trunking Requirements

- Use IEEE 802.1Q VLAN tagging.
- Carry the VLANs required by the network design.
- Retain VLAN 1 temporarily during the initial lab implementation.
- Configure both ends of each physical trunk connection.
- Verify allowed VLANs and trunk operational status.
- Monitor STP behaviour during implementation.
- Review native VLAN and security settings before final approval.

Detailed configuration commands, verification procedures and rollback instructions will be documented before implementation.


#### Pre-Implementation Checks

Before configuring trunk ports, verify the existing VLANs, switch neighbours, and trunk status.

Run the following commands on both core switches:

```text
show vlan brief
show interfaces trunk
show cdp neighbors
show spanning-tree vlan 1
```

Confirm:

- All nine proposed VLANs have been created.
- The interface assignments match the Physical Port Mapping Document.
- The existing STP root configuration is understood.
- A usable backup and rollback procedure are available.

#### Proposed Configuration — CORE-L3-SW1

**Target Interfaces:**
- Gi1/0/1 through Gi1/0/5
- Gi1/0/23

**Configuration Commands:**

```text
enable
configure terminal

interface range GigabitEthernet1/0/1 - 5
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30,40,50,60,70,80,90
 exit

interface GigabitEthernet1/0/23
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30,40,50,60,70,80,90
 exit

end
```

#### Proposed Configuration — CORE-L3-SW2

**Target Interfaces:**
- Gi1/0/1 through Gi1/0/5
- Gi1/0/23

**Configuration Commands:**

```text
enable
configure terminal

interface range GigabitEthernet1/0/1 - 5
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30,40,50,60,70,80,90
 exit

interface GigabitEthernet1/0/23
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30,40,50,60,70,80,90
 exit

end
```

#### Core Switch Trunk Verification

After both ends of the applicable links have been configured, run:

```text
show interfaces trunk
show interfaces GigabitEthernet1/0/1 switchport
show interfaces GigabitEthernet1/0/23 switchport
show spanning-tree vlan 10
```

Verify that:

- Intended interfaces operate as trunks.
- The allowed VLAN list matches the approved design.
- The required VLANs are active and carried across the appropriate links.
- STP remains stable and prevents Layer 2 loops.

Trunk verification must be repeated after the access switches are configured.

**Status:** Commands documented. Pending simulator compatibility checks, review, approval, and execution.


#### Proposed Configuration — Layer 2 Access Switches

**Target Devices:**

- GF-ACC-SW1
- GF-ACC-SW2
- GF-ACC-SW3
- FF-ACC-SW1
- FF-ACC-SW2

**Target Interfaces:** GigabitEthernet0/1 and GigabitEthernet0/2

Each access switch has two physical uplinks:

- Gi0/1 connects to CORE-L3-SW1.
- Gi0/2 connects to CORE-L3-SW2.

#### Proposed Configuration Commands

The following commands will be applied individually to each of the five access switches after pre-implementation checks and approval.

```text
enable
configure terminal

interface range GigabitEthernet0/1 - 2
 switchport mode trunk
 switchport trunk allowed vlan 1,10,20,30,40,50,60,70,80,90
 exit

end
```

#### Access Switch Trunk Verification

After configuring each switch, run:

```text
show interfaces trunk
show interfaces switchport
show cdp neighbors
show spanning-tree vlan 10
```

Verify that:

1. Both GigabitEthernet uplinks are configured as trunks.
2. The allowed VLAN list matches the approved configuration.
3. CDP still identifies the expected core switches.
4. STP prevents switching loops.
5. Required VLANs are carried across the trunk connections.

#### Important Design Considerations

The use of the same VLAN list on all access switches is provisional.

The final design should restrict trunk VLANs to those required at each location.

STP may block some redundant uplinks. A blocked port does not necessarily indicate a fault.

Both ends of each trunk connection must be configured and verified.

**Status:** Access switch trunk commands documented — pending review, approval and implementation.


#### Trunk Configuration Rollback Procedure

**Objective:** Restore the previous working switch configurations if trunk implementation causes unexpected network behaviour.

##### Rollback Triggers

Rollback must be considered if:

- Required VLANs cannot communicate across switches.
- Unexpected trunk or native VLAN mismatches occur.
- STP reports persistent instability or switching loops.
- Critical connectivity is lost after trunk configuration.
- The observed behaviour differs significantly from the approved design.

##### Rollback Steps

1. Stop further configuration changes.
2. Identify the affected switches and interfaces.
3. Record the observed issue and relevant command outputs.
4. Compare the affected interface configurations against the pre-implementation baseline.
5. Restore the previously recorded interface mode, allowed VLAN list and other modified settings.
6. Verify the restored trunk or access-port configuration.
7. Confirm STP stability and expected CDP neighbours.
8. If recovery is unsuccessful, restore the saved Packet Tracer baseline.
9. Document the root cause and corrective action.

##### Rollback Verification Commands

```text
show running-config
show interfaces trunk
show interfaces switchport
show spanning-tree vlan 1
show cdp neighbors
```

Where applicable, repeat STP checks for VLANs 10 through 90.

**Important:** Do not assume that every interface previously operated in access mode. Restore its actual recorded baseline configuration.

Do not disable STP or remove shared VLANs as a general rollback method.

#### Trunk Configuration Success Criteria

The trunk implementation is considered successful when:

- All 11 intended physical links have the correct configuration at both ends.
- Required VLANs are permitted on the appropriate trunks.
- Expected Cisco neighbours remain discoverable using CDP.
- STP maintains a loop-free forwarding topology.
- No unexpected VLAN or trunk mismatches remain.
- Configuration and verification evidence have been recorded.

**Phase Status:** Trunk configuration and rollback procedures documented. Pending final review, approval and implementation.


### 7.4 Access Port and VLAN Assignment

**Implementation Phase:** 5  
**Target Devices:** Five Cisco 2960 Layer 2 access switches  
**Change Status:** Planned — Not Executed

#### Objective

Configure access-switch interfaces to connect end-user devices to their assigned departmental VLANs.

The configuration will ensure that:

- Employees are assigned to the appropriate departmental VLANs.
- Devices belonging to different departments remain logically separated.
- IP phones can use a dedicated voice VLAN where supported.
- Unused switch ports are administratively disabled.
- Sufficient ports remain available for future expansion.

#### Departmental VLAN Assignments

| Department | Floor | VLAN ID | VLAN Name |
|---|---|---|---|
| Sales | Ground | 10 | SALES |
| Operations | Ground and First | 20 | OPERATIONS |
| Finance | First | 30 | FINANCE |
| IT | Ground | 40 | IT |

#### Additional Device VLANs

| Device Category | VLAN ID | VLAN Name |
|---|---|---|
| Servers | 50 | SERVERS |
| IP Phones | 60 | VOICE |
| CCTV Devices | 70 | CCTV |
| Guest Network | 80 | GUEST |
| Network Management | 90 | MANAGEMENT |

The required access-port or uplink configuration for each device category will depend on its connectivity requirements.

#### Access Port Allocation Principles

1. Assign wired employee devices to their designated departmental VLANs.
2. Use FastEthernet access ports for endpoint connections.
3. Reserve GigabitEthernet0/1 and GigabitEthernet0/2 for the existing core-switch uplinks.
4. Allow compatible IP phones and computers to share a physical switch port using separate voice and data VLANs.
5. Administratively disable unused access ports.
6. Reserve available switch-port capacity for future growth.
7. Verify interface assignments before applying configuration changes.

#### Preliminary Sales Port Allocation

The following example is based on the assumed Sales device distribution.

| Port Allocation | Quantity | Purpose |
|---|---|---|
| Active ports | 5 | Wired Sales laptops or docking stations |
| Reserved ports | 5 | Future wired connections |
| Total planned capacity | 10 | Current use and future expansion |

Reserved ports will remain administratively disabled until required.

Exact FastEthernet interface numbers will be determined after reviewing the switch-level device allocation.

#### Implementation Considerations

- The five access switches each provide 24 FastEthernet ports for endpoint connectivity.
- The final port allocation must account for devices on both floors.
- Voice VLAN configuration will only be applied where IP phones are connected.
- Advanced authentication such as IEEE 802.1X is excluded from this initial implementation.
- The final configurations must be verified before saving the switch configurations.

**Phase Status:** Access-port allocation principles documented. Detailed interface assignments and configuration commands pending.


#### Packet Tracer Lab Device Allocation

The following allocation represents a simplified lab deployment.

A limited number of devices will be used to demonstrate VLAN segmentation, inter-VLAN routing, voice VLANs, ACLs, and connectivity testing.

This allocation does not represent all 60 Northstar employees.

| Access Switch | Interface | Planned Device | Data VLAN | Voice VLAN |
|---|---|---|---|---|
| GF-ACC-SW1 | Fa0/1 | Sales PC | 10 | — |
| GF-ACC-SW1 | Fa0/2 | Operations PC | 20 | — |
| GF-ACC-SW1 | Fa0/3 | Sales PC via IP Phone | 10 | 60 |
| GF-ACC-SW2 | Fa0/1 | Guest Test PC | 80 | — |
| GF-ACC-SW3 | Fa0/1 | IT PC | 40 | — |
| GF-ACC-SW3 | Fa0/2 | Test Server | 50 | — |
| GF-ACC-SW3 | Fa0/3 | CCTV Test Device | 70 | — |
| FF-ACC-SW1 | Fa0/1 | Finance PC | 30 | — |
| FF-ACC-SW2 | Fa0/1 | Operations PC | 20 | — |

#### Lab Implementation Notes

- These interface assignments are proposed and have not yet been configured.
- The Sales PC and IP phone will share Fa0/3 if supported by the selected Packet Tracer phone model.
- VLAN 90 will be used for network management rather than a dedicated employee endpoint.
- Gi0/1 and Gi0/2 remain reserved for core-switch trunk connections.
- Unused FastEthernet interfaces will be administratively disabled.
- Device connectivity and VLAN membership will be verified during implementation.
- The full network capacity and 25% growth requirement remain documented in the network design.

**Status:** Representative lab device allocation planned — pending configuration and verification.


#### Proposed Configuration — GF-ACC-SW1

**Device:** GF-ACC-SW1  
**Implementation Status:** Planned — Not Executed

##### Objective

Configure the ground-floor access switch to support Sales and Operations devices, a shared voice/data connection, and basic unused-port security.

##### Proposed Configuration Commands

```text
enable
configure terminal

interface FastEthernet0/1
 description SALES_PC
 switchport mode access
 switchport access vlan 10
 no shutdown
 exit

interface FastEthernet0/2
 description OPERATIONS_PC
 switchport mode access
 switchport access vlan 20
 no shutdown
 exit

interface FastEthernet0/3
 description SALES_PHONE_AND_PC
 switchport mode access
 switchport access vlan 10
 switchport voice vlan 60
 no shutdown
 exit

interface range FastEthernet0/4 - 24
 description UNUSED_RESERVED
 shutdown
 exit

end
```

##### Verification Commands

```text
show vlan brief
show running-config
show interfaces switchport
show interfaces status
```

##### Expected Results

- Fa0/1 is assigned to VLAN 10.
- Fa0/2 is assigned to VLAN 20.
- Fa0/3 uses VLAN 10 for data and VLAN 60 for voice.
- Fa0/4 through Fa0/24 are administratively disabled.
- Gi0/1 and Gi0/2 retain their planned trunk configurations.
- Active endpoint ports can forward traffic once the VLAN and trunk configurations are operational.

##### Implementation Notes

- Confirm the assigned ports are unused before connecting the planned devices.
- Ensure VLANs 10, 20, and 60 exist before configuring the access ports.
- Voice VLAN functionality depends on the selected Packet Tracer IP phone and simulator capabilities.
- Confirm that the IP phone has an appropriate power source.
- Do not shut down any port carrying an active or required connection.
- Record the original interface configurations for rollback.

**Status:** Configuration documented — pending review and implementation.
```

#### Proposed Configuration — GF-ACC-SW2

**Device:** GF-ACC-SW2  
**Implementation Status:** Planned — Not Executed

##### Objective

Configure the ground-floor access switch to provide a dedicated connection for a guest test device in VLAN 80.

Unused access ports will be administratively disabled.

##### Proposed Configuration Commands

```text
enable
configure terminal

interface FastEthernet0/1
 description GUEST_TEST_PC
 switchport mode access
 switchport access vlan 80
 no shutdown
 exit

interface range FastEthernet0/2 - 24
 description UNUSED_RESERVED
 shutdown
 exit

end
```

##### Verification Commands

```text
show vlan brief
show running-config
show interfaces switchport
show interfaces status
```

##### Expected Results

- Fa0/1 is assigned to VLAN 80 (GUEST).
- Fa0/1 is administratively enabled.
- Fa0/2 through Fa0/24 are administratively disabled.
- Gi0/1 and Gi0/2 retain their planned trunk configurations.
- The guest device can communicate within its VLAN once the required connectivity is operational.

##### Implementation Notes

- Ensure VLAN 80 exists before configuring the guest access port.
- The guest device must use the correct IP addressing configuration.
- VLAN assignment alone does not enforce guest network isolation at Layer 3.
- Guest access restrictions will be implemented and tested during the ACL configuration phase.
- Record the original interface configurations for rollback.

**Status:** Configuration documented — pending review and implementation.


#### Proposed Configuration — GF-ACC-SW3

**Device:** GF-ACC-SW3  
**Implementation Status:** Planned — Not Executed

##### Objective

Configure the ground-floor access switch to connect an IT workstation, a test server and a CCTV test device to their respective VLANs.

Unused FastEthernet ports will be administratively disabled.

##### Proposed Configuration Commands

```text
enable
configure terminal

interface FastEthernet0/1
 description IT_PC
 switchport mode access
 switchport access vlan 40
 no shutdown
 exit

interface FastEthernet0/2
 description TEST_SERVER
 switchport mode access
 switchport access vlan 50
 no shutdown
 exit

interface FastEthernet0/3
 description CCTV_TEST_DEVICE
 switchport mode access
 switchport access vlan 70
 no shutdown
 exit

interface range FastEthernet0/4 - 24
 description UNUSED_RESERVED
 shutdown
 exit

end
```

##### Verification Commands

```text
show vlan brief
show running-config
show interfaces switchport
show interfaces status
```

##### Expected Results

- Fa0/1 is assigned to VLAN 40 (IT).
- Fa0/2 is assigned to VLAN 50 (SERVERS).
- Fa0/3 is assigned to VLAN 70 (CCTV).
- Fa0/1 through Fa0/3 are administratively enabled.
- Fa0/4 through Fa0/24 are administratively disabled.
- Gi0/1 and Gi0/2 remain allocated to core-switch trunks.

##### Implementation Notes

- Confirm VLANs 40, 50 and 70 exist before configuring access ports.
- Verify that the correct devices are connected to the assigned interfaces.
- The server and CCTV test device will use IP addresses from their respective VLAN subnets.
- Confirm that the selected CCTV device has a suitable power source, because the simulated 2960-24TT switch does not provide PoE.
- Inter-VLAN connectivity will be tested after the Layer 3 switch configuration.
- Record original interface settings for rollback.

**Status:** Configuration documented — pending review and implementation.


#### Proposed Configuration — FF-ACC-SW1

**Device:** FF-ACC-SW1  
**Implementation Status:** Planned — Not Executed

##### Objective

Configure the first-floor access switch to connect a Finance workstation to VLAN 30.

Unused FastEthernet ports will be administratively disabled.

##### Proposed Configuration Commands

```text
enable
configure terminal

interface FastEthernet0/1
 description FINANCE_PC
 switchport mode access
 switchport access vlan 30
 no shutdown
 exit

interface range FastEthernet0/2 - 24
 description UNUSED_RESERVED
 shutdown
 exit

end
```

##### Verification Commands

```text
show vlan brief
show running-config
show interfaces switchport
show interfaces status
```

##### Expected Results

- Fa0/1 is assigned to VLAN 30 (FINANCE).
- Fa0/1 is administratively enabled.
- Fa0/2 through Fa0/24 are administratively disabled.
- Gi0/1 and Gi0/2 remain allocated to the core-switch trunks.
- The Finance PC can communicate within its VLAN once the required network configuration is operational.

##### Implementation Notes

- Confirm that VLAN 30 exists before configuring the access port.
- Verify that the Finance PC is connected to Fa0/1.
- Assign the Finance PC an IP address from the 192.168.30.0/27 subnet.
- Inter-VLAN routing will be configured on the Layer 3 core switches.
- Finance access restrictions will be implemented and tested during the ACL configuration phase.
- Record original interface settings for rollback.

**Status:** Configuration documented — pending review and implementation.


#### Proposed Configuration — FF-ACC-SW2

**Device:** FF-ACC-SW2  
**Implementation Status:** Planned — Not Executed

##### Objective

Configure the first-floor access switch to connect an Operations workstation to VLAN 20.

The configuration will support connectivity between Operations devices located on different floors through the existing trunk infrastructure.

Unused FastEthernet ports will be administratively disabled.

##### Proposed Configuration Commands

```text
enable
configure terminal

interface FastEthernet0/1
 description OPERATIONS_PC
 switchport mode access
 switchport access vlan 20
 no shutdown
 exit

interface range FastEthernet0/2 - 24
 description UNUSED_RESERVED
 shutdown
 exit

end
```

##### Verification Commands

```text
show vlan brief
show running-config
show interfaces switchport
show interfaces status
```

##### Expected Results

- Fa0/1 is assigned to VLAN 20 (OPERATIONS).
- Fa0/1 is administratively enabled.
- Fa0/2 through Fa0/24 are administratively disabled.
- Gi0/1 and Gi0/2 remain allocated to the core-switch trunks.
- The first-floor Operations PC can communicate with the ground-floor Operations PC when both devices have valid IP configurations and VLAN 20 connectivity is operational.

##### Implementation Notes

- Confirm that VLAN 20 exists before configuring the access port.
- Verify that the Operations PC is connected to Fa0/1.
- Assign the Operations PC an IP address from the 192.168.20.0/26 subnet.
- Ensure VLAN 20 is permitted across the required trunk connections.
- Verify connectivity between the ground-floor and first-floor Operations PCs.
- Record original interface settings for rollback.

**Status:** Configuration documented — pending review and implementation.


#### Access Port Configuration Rollback Procedure

##### Objective

Restore the previous working access-port configurations if VLAN assignments or interface changes cause connectivity problems.

##### Rollback Triggers

Rollback should be considered if:

- A device is assigned to the incorrect VLAN.
- A previously working device loses connectivity.
- An active interface is accidentally disabled.
- A voice VLAN configuration causes unexpected connectivity issues.
- The implemented configuration differs from the approved port allocation.

##### Rollback Steps

1. Stop further configuration changes.
2. Identify the affected switch and interface.
3. Record the issue and relevant command outputs.
4. Compare the interface configuration with the pre-implementation baseline.
5. Restore the original VLAN assignment and interface settings.
6. Restore the original administrative state of the interface.
7. Verify the device's connectivity.
8. If necessary, restore the saved Packet Tracer baseline.
9. Document the cause of the issue and the corrective action.

##### Rollback Verification Commands

```text
show vlan brief
show running-config
show interfaces switchport
show interfaces status
```

Do not remove VLANs or shut down additional ports during rollback unless the original configuration requires it.

#### Access Port Configuration Success Criteria

The access-port implementation is successful when:

- All nine planned endpoint connections use the correct access ports.
- Each device is assigned to its intended data VLAN.
- The shared Sales phone/PC connection uses VLAN 10 for data and VLAN 60 for voice, if supported.
- Unused FastEthernet ports are administratively disabled.
- Core-switch trunk interfaces remain unaffected.
- Same-VLAN connectivity tests succeed.
- Configuration evidence and verification results are recorded.

**Phase Status:** Access-port configurations, verification procedures and rollback steps documented. Pending review and implementation.


### 7.5 Inter-VLAN Routing and SVI Configuration

**Implementation Phase:** 6  
**Target Devices:** Cisco 3650 Layer 3 core switches  
**Change Status:** Planned — Not Executed

#### Objective

Enable communication between the network's VLANs using Switch Virtual Interfaces (SVIs) and Layer 3 routing.

Each VLAN will have a logical Layer 3 gateway that allows devices to communicate with other IP networks, subject to the approved access control policies.

#### Proposed SVI Addressing Plan

| VLAN ID | VLAN Name | Network | Proposed Default Gateway |
|---|---|---|---|
| 10 | SALES | 192.168.10.0/27 | 192.168.10.1 |
| 20 | OPERATIONS | 192.168.20.0/26 | 192.168.20.1 |
| 30 | FINANCE | 192.168.30.0/27 | 192.168.30.1 |
| 40 | IT | 192.168.40.0/28 | 192.168.40.1 |
| 50 | SERVERS | 192.168.50.0/27 | 192.168.50.1 |
| 60 | VOICE | 192.168.60.0/26 | 192.168.60.1 |
| 70 | CCTV | 192.168.70.0/28 | 192.168.70.1 |
| 80 | GUEST | 192.168.80.0/27 | 192.168.80.1 |
| 90 | MANAGEMENT | 192.168.90.0/27 | 192.168.90.1 |

**Gateway Implementation Note:**

The .1 addresses represent the intended final HSRP virtual gateways.

During initial inter-VLAN routing tests, devices will use CORE-L3-SW1's .2 SVI addresses as their default gateways.

After HSRP is successfully implemented on VLANs 10 and 20, devices in those VLANs will use their respective .1 virtual gateway addresses.

Devices in VLANs 30 through 90 will continue using CORE-L3-SW1's .2 gateway addresses until HSRP is implemented and verified for those VLANs.

The proposed addressing table represents the final design and does not indicate that HSRP has already been implemented.

#### Design Considerations

- Layer 3 routing will be performed by the core-switch infrastructure.
- Each VLAN must have a valid gateway address within its assigned subnet.
- Routing between VLANs will require the `ip routing` feature.
- SVI operational status depends on VLAN availability and active Layer 2 connectivity.
- Access Control Lists (ACLs) will restrict communication between designated VLANs.
- The proposed default gateway addresses must remain unique and consistent across the design.
- Gateway redundancy will be evaluated separately after confirming Packet Tracer support for the selected FHRP.

#### Pre-Implementation Requirements

Before configuring SVIs:

1. Confirm all nine VLANs have been created.
2. Confirm the required 802.1Q trunk links are operational.
3. Verify the STP topology is stable.
4. Confirm the IP addressing plan contains no overlapping networks.
5. Verify that the Layer 3 switch supports the required routing commands.
6. Record the existing switch configuration and confirm a usable backup.

Phase Status: SVI addressing, proposed configurations, verification and rollback procedures documented. Pending simulator compatibility checks, implementation and testing.


#### Initial Layer 3 Gateway Design


Both CORE-L3-SW1 and CORE-L3-SW2 will provide Layer 3 routing for the nine configured VLANs.

Each core switch will have a unique physical SVI address:

- CORE-L3-SW1 will use the .2 address in each VLAN.
- CORE-L3-SW2 will use the .3 address in each VLAN.
- The .1 address will be reserved for the future HSRP virtual default gateway.

During initial testing, devices in VLANs without HSRP will use CORE-L3-SW1's .2 address as their default gateway.

HSRP will initially be implemented and tested on VLANs 10 and 20.

The configuration will then be extended to the remaining VLANs if simulator support and initial testing are successful.

Both switches must have the necessary VLAN connectivity and routing configuration before HSRP failover testing.

The same physical SVI IP address must never be assigned to both core switches.


#### Proposed Configuration — CORE-L3-SW1

**Device:** CORE-L3-SW1  
**Implementation Status:** Planned — Not Executed

##### Objective

Enable IP routing and configure Switch Virtual Interfaces (SVIs) to provide default gateways for the nine departmental and service VLANs.

##### Proposed Configuration Commands

```text
enable
configure terminal

ip routing

interface Vlan10
 description SALES_GATEWAY
 ip address 192.168.10.2 255.255.255.224
 no shutdown
 exit

interface Vlan20
 description OPERATIONS_GATEWAY
 ip address 192.168.20.2 255.255.255.192
 no shutdown
 exit

interface Vlan30
 description FINANCE_GATEWAY
 ip address 192.168.30.2 255.255.255.224
 no shutdown
 exit

interface Vlan40
 description IT_GATEWAY
 ip address 192.168.40.2 255.255.255.240
 no shutdown
 exit

interface Vlan50
 description SERVERS_GATEWAY
 ip address 192.168.50.2 255.255.255.224
 no shutdown
 exit

interface Vlan60
 description VOICE_GATEWAY
 ip address 192.168.60.2 255.255.255.192
 no shutdown
 exit

interface Vlan70
 description CCTV_GATEWAY
 ip address 192.168.70.2 255.255.255.240
 no shutdown
 exit

interface Vlan80
 description GUEST_GATEWAY
 ip address 192.168.80.2 255.255.255.224
 no shutdown
 exit

interface Vlan90
 description MANAGEMENT_GATEWAY
 ip address 192.168.90.2 255.255.255.224
 no shutdown
 exit

end
```

#### Proposed Configuration — CORE-L3-SW2 Additional SVIs

**Device:** CORE-L3-SW2  
**Implementation Status:** Planned — Not Executed

##### Objective

Configure the remaining seven SVIs on CORE-L3-SW2 to support Layer 3 routing across all nine VLANs.

SVIs for VLANs 10 and 20 are documented separately in Section 7.6 and must also be operational for the final design.

##### Proposed Configuration Commands

```text
enable
configure terminal

ip routing

interface Vlan30
 description FINANCE_SW2
 ip address 192.168.30.3 255.255.255.224
 no shutdown
 exit

interface Vlan40
 description IT_SW2
 ip address 192.168.40.3 255.255.255.240
 no shutdown
 exit

interface Vlan50
 description SERVERS_SW2
 ip address 192.168.50.3 255.255.255.224
 no shutdown
 exit

interface Vlan60
 description VOICE_SW2
 ip address 192.168.60.3 255.255.255.192
 no shutdown
 exit

interface Vlan70
 description CCTV_SW2
 ip address 192.168.70.3 255.255.255.240
 no shutdown
 exit

interface Vlan80
 description GUEST_SW2
 ip address 192.168.80.3 255.255.255.224
 no shutdown
 exit

interface Vlan90
 description MANAGEMENT_SW2
 ip address 192.168.90.3 255.255.255.224
 no shutdown
 exit

end
```

##### Verification Commands

```text
show ip interface brief
show ip route
show running-config
```

##### Expected Results

- All seven additional SVIs have the correct IP addresses and subnet masks.
- The SVIs become operational when their VLANs have the required active Layer 2 connectivity.
- Operational SVIs appear as directly connected networks in the routing table.
- No physical SVI IP addresses conflict with CORE-L3-SW1.
- VLANs 10 and 20 will also be configured on CORE-L3-SW2 as documented in Section 7.6.

**Status:** Additional SVI commands documented — pending review and implementation.


##### Initial Verification Commands

```text
show ip interface brief
show ip route
show running-config
show vlan brief
```

##### Expected Results

- IP routing is enabled on CORE-L3-SW1.
- All nine SVIs have their planned IP addresses and subnet masks.
- Operational VLAN interfaces appear in the IP routing table as directly connected networks.
- Devices can reach their respective default gateways once the required access ports and trunks are operational.
- Inter-VLAN connectivity is available before restrictive ACLs are applied.
- CORE-L3-SW2 does not use duplicate default gateway addresses.

##### Implementation Notes

- Verify that all required VLANs exist before configuring SVIs.
- An SVI may remain down if its VLAN has no operational Layer 2 connectivity.
- Record verification results before implementing ACL restrictions.
- VRRP implementation will be evaluated separately.
- Record the original switch configuration for rollback.

**Status:** Initial Layer 3 SVI configuration documented — pending review and implementation.


#### SVI and Inter-VLAN Routing Rollback Procedure

##### Objective

Restore the previous working Layer 3 configuration if SVI configuration or inter-VLAN routing changes cause network connectivity issues.

##### Rollback Triggers

Rollback should be considered if:

- Devices cannot reach their configured default gateways.
- Incorrect IP addresses or subnet masks are assigned to SVIs.
- Unexpected IP addressing conflicts occur.
- Previously working VLAN connectivity is disrupted.
- The routing table contains unexpected or missing networks.
- The implemented configuration differs from the approved design.

##### Rollback Steps

1. Stop further configuration changes.
2. Identify the affected VLANs and SVI interfaces.
3. Record the observed issue and relevant command outputs.
4. Compare the current configuration with the saved baseline.
5. Restore the original SVI IP addressing and interface settings.
6. Restore the previous IP routing configuration if it was changed.
7. Verify the operational status of the affected SVIs.
8. Confirm that the routing table matches the restored configuration.
9. Test connectivity to the affected default gateways.
10. If recovery is unsuccessful, restore the saved Packet Tracer baseline.
11. Document the root cause and corrective action.

##### Rollback Verification Commands

```text
show ip interface brief
show ip route
show running-config
show vlan brief
show interfaces trunk
```

Where applicable, use ping tests to verify connectivity to the affected gateway addresses.

**Important:** Restore the actual previous configuration. Do not disable IP routing or remove unrelated SVIs as a general rollback method.

#### SVI and Inter-VLAN Routing Success Criteria

The implementation is successful when:

- IP routing is enabled on CORE-L3-SW1.
- All nine SVIs have the correct IP addresses and subnet masks.
- SVIs with the required active Layer 2 connectivity are operational.
- The routing table contains the expected directly connected networks.
- Test devices can reach their respective default gateways.
- Authorised inter-VLAN connectivity tests succeed.
- CORE-L3-SW2 has no conflicting gateway IP addresses.
- Configuration and verification evidence has been recorded.

Inter-VLAN routing will be tested before applying the planned ACL restrictions.

**Phase Status:** SVI configuration, verification and rollback procedures documented. Pending final review and implementation.


### 7.6 Gateway Redundancy and VLAN-Based Load Sharing

**Implementation Phase:** 7  
**Target Devices:** CORE-L3-SW1 and CORE-L3-SW2  
**Change Status:** Planned — Not Executed

#### Objective

Provide redundant default gateways using a First Hop Redundancy Protocol (FHRP).

Distribute active gateway responsibilities between the two core switches to demonstrate VLAN-based load sharing and gateway failover.

#### Protocol Selection

VRRP is included in the network design as a standards-based FHRP.

HSRP is the preferred implementation for this Cisco Packet Tracer lab because VRRP support is unavailable in reported Packet Tracer versions.

Simulator support will be verified before configuration.

#### Proposed HSRP Role Allocation

| VLAN | Active Switch | Standby Switch |
|---|---|---|
| 10 — SALES | CORE-L3-SW1 | CORE-L3-SW2 |
| 20 — OPERATIONS | CORE-L3-SW2 | CORE-L3-SW1 |
| 30 — FINANCE | CORE-L3-SW1 | CORE-L3-SW2 |
| 40 — IT | CORE-L3-SW2 | CORE-L3-SW1 |
| 50 — SERVERS | CORE-L3-SW1 | CORE-L3-SW2 |
| 60 — VOICE | CORE-L3-SW2 | CORE-L3-SW1 |
| 70 — CCTV | CORE-L3-SW1 | CORE-L3-SW2 |
| 80 — GUEST | CORE-L3-SW2 | CORE-L3-SW1 |
| 90 — MANAGEMENT | CORE-L3-SW1 | CORE-L3-SW2 |

#### Gateway Addressing Principles

- The .1 address in each VLAN will be reserved for the virtual default gateway.
- CORE-L3-SW1 will use .2 as its SVI address in each VLAN.
- CORE-L3-SW2 will use .3 as its SVI address in each VLAN.
- Both core switches must have unique physical SVI addresses.
- End devices will use their VLAN's virtual gateway address.

#### STP Alignment

The preferred STP root bridge will align with the active gateway for each VLAN where practical.

STP priorities documented in Section 7.2 will be revised before implementation.

#### Implementation Approach

1. Verify HSRP command support in Packet Tracer.
2. Update the SVI and STP configuration plans.
3. Configure and test gateway redundancy on VLANs 10 and 20 first.
4. Verify active and standby roles.
5. Test gateway failover and recovery.
6. Extend the configuration to the remaining VLANs if the initial tests succeed.
7. Record configuration, verification and troubleshooting evidence.

**Phase Status:** Gateway redundancy design documented. Detailed commands, verification and rollback procedures pending.

#### HSRP Pre-Implementation Checks

Before configuring HSRP, verify the following:

1. Both Layer 3 core switches are operational.
2. VLANs 10 and 20 exist on both core switches.
3. The required trunk connections are operational.
4. STP root bridge priorities match the planned design.
5. IP routing is enabled on both core switches.
6. Physical SVI addresses are unique.
7. The virtual gateway addresses are reserved for HSRP.
8. HSRP commands are supported by the selected Packet Tracer switches.
9. A usable configuration backup is available.

##### Verification Commands

```text
show version
show vlan brief
show ip interface brief
show ip route
show interfaces trunk
show spanning-tree vlan 10
show spanning-tree vlan 20
```

Once HSRP support is confirmed, use:

```text
show standby brief
```

If the simulator does not recognise the HSRP commands, document the limitation and review an alternative supported device or topology.

##### Initial Test VLANs

| VLAN | Virtual Gateway | Preferred Active | Standby |
|---|---|---|---|
| 10 — SALES | 192.168.10.1 | CORE-L3-SW1 | CORE-L3-SW2 |
| 20 — OPERATIONS | 192.168.20.1 | CORE-L3-SW2 | CORE-L3-SW1 |

The initial test will verify gateway redundancy, active and standby roles, and failover behaviour before extending HSRP to the remaining VLANs.

**Status:** HSRP pre-implementation requirements documented — pending simulator verification.


#### Proposed HSRP Configuration — CORE-L3-SW1

**Device:** CORE-L3-SW1  
**Implementation Status:** Planned — Not Executed

##### Objective

Configure CORE-L3-SW1 to act as the preferred HSRP Active gateway for VLAN 10 and the Standby gateway for VLAN 20.

##### Prerequisites

- VLANs 10 and 20 exist.
- IP routing is enabled.
- VLAN 10 SVI address is 192.168.10.2/27.
- VLAN 20 SVI address is 192.168.20.2/26.
- Both SVIs have the required Layer 2 connectivity.
- HSRP command support has been confirmed in Packet Tracer.

##### Proposed Configuration Commands

```text
enable
configure terminal

interface Vlan10
 standby 10 ip 192.168.10.1
 standby 10 priority 110
 standby 10 preempt
 exit

interface Vlan20
 standby 20 ip 192.168.20.1
 standby 20 priority 100
 exit

end
```

##### Expected HSRP Roles

| VLAN | HSRP Group | Virtual IP | Priority | Intended Role |
|---|---|---|---|---|
| 10 | 10 | 192.168.10.1 | 110 | Active |
| 20 | 20 | 192.168.20.1 | 100 | Standby |

##### Verification Commands

```text
show standby brief
show standby
show ip interface brief
```

The expected Active and Standby roles must be verified after both core switches have been configured and their required interfaces are operational.

##### Implementation Notes

- The virtual IP address must not be assigned as a physical SVI address.
- `standby 10 preempt` allows SW1 to reclaim the Active role for VLAN 10 when it returns with a higher priority.
- VLAN 20 will use SW2 as its preferred Active gateway.
- HSRP implementation depends on simulator support.
- Record the actual HSRP states during testing.

**Status:** Initial HSRP configuration for CORE-L3-SW1 documented — pending verification and implementation.


#### Proposed HSRP Configuration — CORE-L3-SW2

**Device:** CORE-L3-SW2  
**Implementation Status:** Planned — Not Executed

##### Objective

Configure CORE-L3-SW2 to act as the preferred HSRP Active gateway for VLAN 20 and the Standby gateway for VLAN 10.

Configure unique physical SVI addresses to allow both core switches to participate in HSRP.

##### Prerequisites

- VLANs 10 and 20 exist on both core switches.
- Trunk connectivity between the core switches is operational.
- The planned STP priorities have been configured and verified.
- HSRP command support has been confirmed in Packet Tracer.
- CORE-L3-SW1 has physical SVI addresses ending in .2.
- Virtual gateway addresses ending in .1 remain reserved for HSRP.

##### Proposed Configuration Commands

```text
enable
configure terminal

ip routing

interface Vlan10
 description SALES_HSRP_SW2
 ip address 192.168.10.3 255.255.255.224
 no shutdown
 standby 10 ip 192.168.10.1
 standby 10 priority 100
 exit

interface Vlan20
 description OPERATIONS_HSRP_SW2
 ip address 192.168.20.3 255.255.255.192
 no shutdown
 standby 20 ip 192.168.20.1
 standby 20 priority 110
 standby 20 preempt
 exit

end
```

##### Expected HSRP Roles

| VLAN | HSRP Group | Virtual IP | Priority | Intended Role |
|---|---|---|---|---|
| 10 | 10 | 192.168.10.1 | 100 | Standby |
| 20 | 20 | 192.168.20.1 | 110 | Active |

##### Verification Commands

```text
show standby brief
show standby
show ip interface brief
show ip route
```

##### Expected Results

- CORE-L3-SW2 has unique physical SVI addresses for VLANs 10 and 20.
- VLAN 10 operates in the Standby state when CORE-L3-SW1 is available and Active.
- VLAN 20 operates in the Active state when both switches are available.
- Both switches advertise the same virtual gateway address within each HSRP group.
- No duplicate physical IP addresses exist.
- HSRP states and priorities match the planned design.

##### Implementation Notes

- The `standby 20 preempt` command allows CORE-L3-SW2 to reclaim the Active role for VLAN 20 after recovery.
- CORE-L3-SW1 remains the preferred Active gateway for VLAN 10.
- CORE-L3-SW2 will have SVIs for all nine VLANs. HSRP will initially be configured and tested on VLANs 10 and 20 only.
- Redundancy for the remaining VLANs will require additional HSRP configurations on both core switches, followed by failover testing.
- Actual HSRP operation must be verified in Packet Tracer.
- Record the original configuration for rollback.

**Status:** Initial HSRP configuration for CORE-L3-SW2 documented — pending review and implementation.


#### HSRP Failover Testing Procedure

**Implementation Status:** Planned — Not Executed

##### Objective

Verify that HSRP provides gateway redundancy for VLANs 10 and 20.

Confirm that the Standby switch assumes the Active role when the preferred gateway becomes unavailable.

##### Test 1 — Verify Initial HSRP States

On both core switches, run:

```text
show standby brief
show standby
```

Expected results:

- CORE-L3-SW1 is Active for VLAN 10.
- CORE-L3-SW2 is Standby for VLAN 10.
- CORE-L3-SW2 is Active for VLAN 20.
- CORE-L3-SW1 is Standby for VLAN 20.

Record the initial states and priorities.

##### Test 2 — VLAN 10 Gateway Failover

Before the test, verify that the Sales test PC can reach the virtual gateway at 192.168.10.1.

On CORE-L3-SW1, simulate loss of the VLAN 10 gateway:

```text
enable
configure terminal
interface Vlan10
 shutdown
end
```

Verify the HSRP state on CORE-L3-SW2:

```text
show standby brief
```

Expected result:

- CORE-L3-SW2 becomes Active for VLAN 10.
- The Sales PC regains connectivity to the virtual gateway after HSRP convergence.

Restore the VLAN 10 SVI on CORE-L3-SW1:

```text
configure terminal
interface Vlan10
 no shutdown
end
```

Verify that CORE-L3-SW1 reclaims the Active role through preemption.

##### Test 3 — VLAN 20 Gateway Failover

Before the test, verify that the Operations test PC can reach the virtual gateway at 192.168.20.1.

On CORE-L3-SW2, simulate loss of the VLAN 20 gateway:

```text
enable
configure terminal
interface Vlan20
 shutdown
end
```

Verify the HSRP state on CORE-L3-SW1:

```text
show standby brief
```

Expected result:

- CORE-L3-SW1 becomes Active for VLAN 20.
- The Operations PC regains connectivity to the virtual gateway after HSRP convergence.

Restore the VLAN 20 SVI on CORE-L3-SW2:

```text
configure terminal
interface Vlan20
 no shutdown
end
```

Verify that CORE-L3-SW2 reclaims the Active role through preemption.

##### Test Evidence

For each test, record:

1. Initial Active and Standby roles.
2. HSRP states after the simulated failure.
3. Connectivity test results before and after failover.
4. HSRP states after restoration.
5. Any unexpected behaviour or troubleshooting performed.

##### Success Criteria

- Both VLANs elect the intended Active and Standby gateways.
- The Standby switch successfully assumes the Active role when required.
- End devices regain gateway connectivity after convergence.
- The preferred Active gateway reclaims its role after recovery.
- No duplicate physical SVI addresses are present.
- Verification evidence is recorded.

**Status:** HSRP failover tests documented — pending implementation and verification.


#### HSRP Configuration Rollback Procedure

**Implementation Status:** Planned — Not Executed

##### Objective

Restore the previous working gateway configuration if HSRP implementation causes unexpected connectivity or redundancy issues.

##### Rollback Triggers

Rollback should be considered if:

- HSRP Active and Standby roles do not match the approved design.
- End devices cannot reach their virtual default gateways.
- HSRP failover does not occur as expected.
- Unexpected virtual gateway or physical SVI address conflicts occur.
- Routing or connectivity is disrupted after implementing HSRP.
- Gateway recovery fails after restoring the original Active switch.

##### Rollback Steps

1. Stop further configuration changes.
2. Identify the affected VLAN, HSRP group and core switch.
3. Record the current HSRP states, priorities and virtual IP addresses.
4. Compare the configuration against the saved pre-implementation baseline.
5. Restore the previously recorded HSRP settings or remove the newly introduced HSRP configuration if necessary.
6. Restore the original physical SVI configuration where required.
7. If reverting to the initial non-redundant routing design, restore the appropriate endpoint default gateway addresses.
8. Verify the affected SVIs and IP routing table.
9. Test connectivity to the restored default gateways.
10. If recovery is unsuccessful, restore the saved Packet Tracer baseline.
11. Document the root cause and corrective action.

##### Rollback Verification Commands

```text
show standby brief
show standby
show ip interface brief
show ip route
show running-config
```

If HSRP has been removed, the HSRP verification commands may no longer display active groups.

##### Important Considerations

- Do not assign the HSRP virtual IP address to both physical SVIs.
- Do not remove unrelated VLANs or trunk configurations.
- Restore the actual pre-implementation configuration rather than assuming a particular gateway address.
- Confirm that endpoint default gateways match the restored routing design.
- Verify both Layer 2 and Layer 3 connectivity after rollback.

#### HSRP Configuration Success Criteria

The implementation is considered successful when:

- CORE-L3-SW1 is Active for VLAN 10 and Standby for VLAN 20.
- CORE-L3-SW2 is Active for VLAN 20 and Standby for VLAN 10.
- Both VLANs use the correct virtual default gateway addresses.
- Physical SVI IP addresses are unique.
- Both HSRP failover tests succeed.
- Preemption restores the preferred Active gateway roles.
- End-to-end connectivity is restored after convergence.
- Configuration and troubleshooting evidence is recorded.

Additional VLANs will be configured and verified only after the initial HSRP tests succeed.

**Phase Status:** HSRP configuration, verification, failover testing and rollback procedures documented. Pending simulator verification, implementation and testing.


### 7.7 Access Control List (ACL) Configuration

**Implementation Phase:** 8  
**Target Devices:** CORE-L3-SW1 and CORE-L3-SW2  
**Change Status:** Planned — Not Executed

#### Objective

Implement extended Access Control Lists (ACLs) to control traffic between VLANs and demonstrate basic network access restrictions.

The configuration will focus on protecting the Finance network and preventing Guest devices from accessing internal company networks.

#### Proposed Access Control Policy

| Source VLAN | Destination | Action |
|---|---|---|
| VLAN 10 — SALES | VLAN 30 — FINANCE | Deny |
| VLAN 80 — GUEST | Internal company VLANs | Deny |
| VLAN 40 — IT | VLAN 30 — FINANCE | Permit |
| VLAN 20 — OPERATIONS | VLAN 50 — SERVERS | Permit |

These are initial lab policies. Any additional Finance access restrictions will be reviewed before implementation.

#### ACL Design Principles

1. Use extended IPv4 ACLs to identify source and destination networks.
2. Apply ACLs in the appropriate direction on the relevant Layer 3 interfaces.
3. Order ACL entries carefully because they are evaluated from top to bottom.
4. Account for the implicit deny statement at the end of each ACL.
5. Maintain necessary authorised connectivity.
6. Apply equivalent restrictions to both core switches where required by the HSRP design.
7. Verify connectivity before and after applying the ACLs.
8. Record the original configuration for rollback.

#### Planned Connectivity Tests

- Verify that Sales cannot access the Finance test PC.
- Verify that Guest cannot access the internal test server.
- Verify that IT can access the Finance test PC.
- Verify that Operations can access the test server.
- Verify that permitted traffic continues working after ACL implementation.
- Repeat relevant tests after HSRP gateway failover.

#### Implementation Considerations

- Inter-VLAN routing must be operational before ACL deployment.
- ACL rules must be reviewed to avoid accidentally blocking required traffic.
- Guest internet access cannot be verified until internet connectivity is included in the lab.
- Configuration commands, verification procedures and rollback steps will be documented before implementation.

**Phase Status:** Initial ACL policy documented. Detailed configuration and verification procedures pending.


#### Proposed ACL Configuration — Sales to Finance

**Target Devices:** CORE-L3-SW1 and CORE-L3-SW2  
**Implementation Status:** Planned — Not Executed

##### Objective

Prevent Sales devices in VLAN 10 from accessing the Finance network in VLAN 30 while allowing other authorised traffic.

##### Proposed Configuration Commands

Apply the following configuration to both core switches after confirming their VLAN 10 SVIs are operational.

```text
enable
configure terminal

ip access-list extended SALES_VLAN10_IN
 deny ip 192.168.10.0 0.0.0.31 192.168.30.0 0.0.0.31
 permit ip any any
 exit

interface Vlan10
 ip access-group SALES_VLAN10_IN in
 exit

end
```

##### Verification Commands

```text
show ip access-lists
show ip interface Vlan10
show running-config
```

##### Expected Results

- Sales devices cannot access the Finance subnet.
- Sales devices can access other permitted networks.
- IT devices can still access Finance.
- The ACL is applied inbound on VLAN 10.
- Both core switches enforce the same restriction.
- The restriction remains effective after HSRP gateway failover.

##### Implementation Notes

- Confirm inter-VLAN routing works before applying the ACL.
- The wildcard mask 0.0.0.31 corresponds to a /27 subnet.
- ACL entries are evaluated from top to bottom.
- The final permit statement prevents the implicit deny from blocking unrelated IP traffic.
- This ACL restricts Sales-to-Finance traffic only; it is not a complete Finance security policy.
- Record the original configuration for rollback.
- Confirm that the selected Packet Tracer switches support the ACL commands.

**Status:** Sales-to-Finance ACL documented — pending review and implementation.


#### Proposed ACL Configuration — Guest Network Isolation

**Target Devices:** CORE-L3-SW1 and CORE-L3-SW2  
**Implementation Status:** Planned — Not Executed

##### Objective

Prevent devices in VLAN 80 (GUEST) from accessing Northstar's internal company VLANs.

Allow other IP traffic that is not explicitly denied.

##### Proposed Configuration Commands

Configure the following ACL on each core switch once its VLAN 80 SVI is configured and operational.

```text
enable
configure terminal

ip access-list extended GUEST_VLAN80_IN
 deny ip 192.168.80.0 0.0.0.31 192.168.10.0 0.0.0.31
 deny ip 192.168.80.0 0.0.0.31 192.168.20.0 0.0.0.63
 deny ip 192.168.80.0 0.0.0.31 192.168.30.0 0.0.0.31
 deny ip 192.168.80.0 0.0.0.31 192.168.40.0 0.0.0.15
 deny ip 192.168.80.0 0.0.0.31 192.168.50.0 0.0.0.31
 deny ip 192.168.80.0 0.0.0.31 192.168.60.0 0.0.0.63
 deny ip 192.168.80.0 0.0.0.31 192.168.70.0 0.0.0.15
 deny ip 192.168.80.0 0.0.0.31 192.168.90.0 0.0.0.31
 permit ip any any
 exit

interface Vlan80
 ip access-group GUEST_VLAN80_IN in
 exit

end
```

##### Verification Commands

```text
show ip access-lists
show ip interface Vlan80
show running-config
```

##### Expected Results

- Guest devices cannot access the Sales network.
- Guest devices cannot access the Operations network.
- Guest devices cannot access the Finance network.
- Guest devices cannot access the IT network.
- Guest devices cannot access the Servers network.
- Guest devices cannot access the Voice network.
- Guest devices cannot access the CCTV network.
- Guest devices cannot access the Management network.
- Traffic not matching the deny statements is permitted by this ACL.
- The same Guest restrictions remain effective after HSRP failover, once both switches have the required VLAN 80 configurations.

##### Implementation Notes

- Confirm inter-VLAN routing works before applying the ACL.
- The Guest subnet is 192.168.80.0/27.
- Apply the ACL inbound on the VLAN 80 SVI.
- Configure the ACL on CORE-L3-SW1 initially.
- Apply the equivalent configuration on CORE-L3-SW2 after its VLAN 80 SVI is implemented.
- The final permit statement does not guarantee internet access; internet routing is outside the initial lab scope.
- Test permitted and denied traffic after implementation.
- Record the original configuration for rollback.

**Status:** Guest network isolation ACL documented — pending review and implementation.


#### ACL Configuration Rollback Procedure

**Implementation Status:** Planned — Not Executed

##### Objective

Restore the previous working network configuration if ACL implementation causes unexpected connectivity problems.

##### Rollback Triggers

Rollback should be considered if:

- Authorised devices lose access to required networks.
- An ACL blocks traffic that should be permitted.
- An incorrect source or destination network is configured.
- ACL restrictions behave differently after HSRP failover.
- The implemented configuration differs from the approved security policy.

##### Rollback Steps

1. Stop further configuration changes.
2. Identify the affected VLAN, SVI and ACL.
3. Record the observed connectivity problem.
4. Review the ACL entries and their sequence.
5. Compare the configuration against the saved baseline.
6. Restore the previous ACL entries and interface assignments.
7. Remove newly introduced ACL bindings where necessary.
8. Verify that legitimate connectivity has been restored.
9. Confirm that any remaining security restrictions behave as intended.
10. Restore the saved Packet Tracer baseline if recovery is unsuccessful.
11. Document the root cause and corrective action.

##### Example — Removing a Newly Applied ACL

The following example removes the Guest ACL from the VLAN 80 SVI without deleting the ACL definition.

```text
enable
configure terminal

interface Vlan80
 no ip access-group GUEST_VLAN80_IN in
 exit

end
```

Only use this example when the ACL was newly applied and the approved rollback requires its removal.

Do not permanently remove required security controls without reviewing the impact.

##### Rollback Verification Commands

```text
show ip access-lists
show ip interface Vlan10
show ip interface Vlan80
show running-config
show ip route
```

Repeat the affected connectivity tests after restoring the configuration.

#### ACL Configuration Success Criteria

The ACL implementation is considered successful when:

- Sales devices cannot access the Finance subnet.
- Guest devices cannot access the eight designated internal VLANs.
- IT devices can access the Finance test PC.
- Operations devices can access the internal test server.
- Other authorised network communication remains operational.
- ACLs are applied to the correct SVI interfaces and directions.
- The restrictions remain effective after applicable HSRP failover tests.
- Configuration and test evidence have been recorded.

##### Test Evidence

Record:

1. Source and destination IP addresses.
2. Expected connectivity result.
3. Actual connectivity result.
4. Relevant ACL configuration and verification output.
5. Troubleshooting actions if the results differ.

**Phase Status:** ACL configurations, verification procedures, rollback steps and success criteria documented. Pending implementation and testing.


### 7.8 End-to-End Connectivity Testing

**Implementation Phase:** 9  
**Target Devices:** Core switches, access switches and simulated endpoints  
**Change Status:** Planned — Not Executed

#### Objective

Verify that VLAN segmentation, trunking, inter-VLAN routing, HSRP and ACL security policies operate as intended.

Record connectivity results and investigate any unexpected failures.

#### Test 1 — Layer 2 Connectivity

Verify connectivity between the ground-floor and first-floor Operations PCs.

Both devices belong to VLAN 20 and should communicate through the trunk infrastructure without requiring inter-VLAN routing.

#### Test 2 — Default Gateway Connectivity

Verify that test devices can reach their assigned default gateways.

After HSRP implementation, verify that:

- Sales devices use 192.168.10.1.
- Operations devices use 192.168.20.1.
- Other devices use their approved VLAN gateway addresses.

#### Test 3 — Inter-VLAN Routing

Before implementing restrictive ACLs, verify that routing works between the required VLANs.

Confirm that both core switches have the necessary routes to reach destination networks.

#### Test 4 — ACL Security Verification

| Source | Destination | Expected Result |
|---|---|---|
| Sales PC | Finance PC | Denied |
| Guest PC | Test Server | Denied |
| IT PC | Finance PC | Permitted |
| Operations PC | Test Server | Permitted |

#### Test 5 — HSRP Failover Verification

Verify that:

- CORE-L3-SW1 is initially Active for VLAN 10.
- CORE-L3-SW2 is initially Active for VLAN 20.
- The Standby gateway takes over when the Active gateway becomes unavailable.
- Connectivity recovers after HSRP convergence.
- ACL restrictions remain effective after failover.

#### Verification Commands

On the relevant Cisco switches:

```text
show vlan brief
show interfaces trunk
show spanning-tree
show ip interface brief
show ip route
show standby brief
show ip access-lists
```

Use ping tests from the simulated endpoints to verify IP connectivity.

#### Test Evidence

For each test, record:

1. Source and destination devices.
2. Source and destination IP addresses.
3. Expected result.
4. Actual result.
5. Relevant command output or screenshot.
6. Troubleshooting performed, if necessary.

#### Success Criteria

The implementation is successful when:

- Same-VLAN connectivity works across the access switches.
- Test devices can reach their assigned gateways.
- Authorised inter-VLAN communication succeeds.
- ACLs block the intended unauthorised traffic.
- HSRP failover works on the supported test VLANs.
- No unexpected connectivity failures remain.
- Test evidence has been recorded.

**Phase Status:** End-to-end testing procedures documented — pending implementation and verification.


### 7.9 Final Verification and Configuration Save

**Implementation Phase:** 10  
**Target Devices:** All Cisco switches and simulated endpoints  
**Change Status:** Planned — Not Executed

#### Objective

Complete the final verification of the implemented network configuration.

Confirm that the network meets the approved technical requirements before saving the final Packet Tracer project and recording implementation evidence.

#### Final Configuration Verification

Verify the following:

1. All nine VLANs have been created correctly.
2. The 11 physical inter-switch connections have the intended trunk configurations.
3. STP root bridge roles match the approved design.
4. Access ports belong to their assigned VLANs.
5. Unused access ports are administratively disabled.
6. All required SVIs have valid IP addresses and subnet masks.
7. Both core switches have the necessary routes for their assigned routing responsibilities.
8. HSRP operates correctly on the implemented test VLANs.
9. ACLs enforce the required security restrictions.
10. All required connectivity tests have been completed.

#### Final Verification Commands

Run the relevant commands on each switch:

```text
show running-config
show vlan brief
show interfaces trunk
show spanning-tree
show ip interface brief
show ip route
show standby brief
show ip access-lists
show cdp neighbors
```

Commands that are not supported or applicable on a particular device must be recorded as such.

#### Configuration Save Procedure

Once the configurations and verification results have been reviewed, save the running configurations on the relevant Cisco switches.

```text
copy running-config startup-config
```

Confirm that the save operation completes successfully.

#### Packet Tracer Project Save

1. Preserve the original topology baseline.
2. Save the completed implementation as a new Packet Tracer project version.
3. Use a descriptive filename identifying the project and version.
4. Confirm that the saved project opens successfully.
5. Retain relevant configuration and test evidence.

#### Final Evidence Requirements

Record:

- The completed network topology.
- VLAN and trunk verification outputs.
- STP root bridge verification.
- SVI addresses and routing table outputs.
- HSRP Active and Standby states.
- ACL configuration and connectivity test results.
- Any troubleshooting performed.
- Known simulator limitations or unimplemented features.

#### Final Success Criteria

The implementation is considered successful when:

- All implemented features operate according to the approved design.
- Required authorised connectivity succeeds.
- Intended security restrictions are enforced.
- Redundancy tests succeed for the supported configurations.
- No unresolved critical configuration errors remain.
- Configuration backups and the final Packet Tracer file are saved.
- Verification evidence is available for project documentation.

Any incomplete or unsupported feature must be clearly documented rather than reported as successfully implemented.

**Phase Status:** Final verification and configuration-save procedures documented. Pending implementation and completion of testing.
