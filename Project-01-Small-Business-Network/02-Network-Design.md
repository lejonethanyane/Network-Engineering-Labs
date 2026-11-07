
# Project 01 — Network Design Document

**Client:** Northstar Business Solutions (Fictional)  
**Location:** Johannesburg, South Africa  
**Environment:** Cisco Packet Tracer  
**Document Status:** Draft — Pending Design Review  

---

## 1. Network Design Overview

This document describes the proposed network architecture for Northstar Business Solutions.

The design is based on the business requirements recorded in `01-Requirements.md`.

The objective is to create a secure, scalable and reliable network supporting 60 employees across two office floors, with capacity for 25% employee growth.

## 2. Network Design Objectives

The proposed network must:

1. Provide wired and wireless connectivity across both floors.
2. Support departmental network segmentation using VLANs.
3. Enable controlled inter-VLAN communication.
4. Restrict unauthorised access to Finance resources.
5. Provide isolated guest internet access.
6. Support internal servers and cloud applications.
7. Accommodate future employee and device growth.
8. Evaluate redundancy for critical network services.
9. Remain within the allocated R350,000 project budget.

## 3. Proposed Logical Network Architecture

The preliminary architecture consists:

- An internet connection provided by an ISP.
- An edge firewall/router for internet connectivity and security.
- A core switching layer connecting the internal network.
- Access switches serving users and devices on each floor.
- Internal servers connected through an appropriately secured network segment.
- A planned inter-floor uplink connecting the first-floor network to the main communications room.

The final topology, equipment, routing design and redundancy requirements will be evaluated in subsequent sections.

**Design status:** Preliminary proposal. No equipment has been selected or approved.

## 4. Proposed VLAN Segmentation

Northstar Business Solutions will use VLAN segmentation to logically separate departments and network services.

### Preliminary VLAN Groups

| VLAN Group | Purpose |
|---|---|
| Sales | Sales department devices |
| Operations | Operations department devices |
| Finance | Restricted Finance department devices |
| IT | IT department workstations |
| Servers | Internal business services |
| Voice | IP phones |
| CCTV | Security cameras |
| Guest Wi-Fi | Internet-only guest access |
| Network Management | Management access to network infrastructure |

### Design Justification

- Separate departmental traffic into logical networks.
- Support communication between authorised devices.
- Restrict unauthorised access to Finance systems.
- Isolate guest devices from internal business resources.
- Separate voice, CCTV and infrastructure management traffic.
- Enable controlled inter-VLAN routing through ACLs or firewall policies.

### Design Considerations

VLAN segmentation alone does not enforce complete network isolation.

Inter-VLAN routing and access-control policies must be configured to restrict communication between network segments.

VLAN IDs, subnet assignments and detailed access policies will be defined during the next design stage.

**Status:** Preliminary design — pending review.

### Proposed VLAN ID Allocation

| VLAN ID | VLAN Name | Purpose |
|---|---|---|
| 10 | SALES | Sales department |
| 20 | OPERATIONS | Operations department |
| 30 | FINANCE | Finance department |
| 40 | IT | IT department |
| 50 | SERVERS | Internal servers |
| 60 | VOICE | IP telephony |
| 70 | CCTV | Security cameras |
| 80 | GUEST | Guest wireless connectivity |
| 90 | MANAGEMENT | Network infrastructure management |

**Design Decision:** Allocate separate VLANs to departments and network services to support logical segmentation and controlled communication.

**Note:** VLAN IDs are preliminary. Whether departmental VLANs should span multiple floors will be evaluated during physical and logical topology design.

## 5. Preliminary IP Addressing Plan

All nine proposed VLANs will use private IPv4 addressing.

Subnet sizes have been selected based on current employee numbers, projected 25% growth, and preliminary gateway requirements.

### VLAN IP Address Allocation

| VLAN ID | Department | Network Address | Subnet Mask | Usable Hosts |
|---|---|---|---|---|
| 10 | Sales | 192.168.10.0/27 | 255.255.255.224 | 30 |
| 20 | Operations | 192.168.20.0/26 | 255.255.255.192 | 62 |
| 30 | Finance | 192.168.30.0/27 | 255.255.255.224 | 30 |
| 40 | IT | 192.168.40.0/28 | 255.255.255.240 | 14 |
| 50 | Servers | 192.168.50.0/27 | 255.255.255.224 | 30 |
| 60 | Voice | 192.168.60.0/26 | 255.255.255.192 | 62 |
| 70 | CCTV | 192.168.70.0/28 | 255.255.255.240 | 14 |
| 80 | Guest Wi-Fi | 192.168.80.0/27 | 255.255.255.224 | 30 |
| 90 | Network Management | 192.168.90.0/27 | 255.255.255.224 | 30 |

### Design Considerations

- Each departmental VLAN will have a separate IPv4 subnet.
- Subnet sizes account for projected employee growth.
- Default gateway addresses will be finalised during the routing design.
- Additional gateway addresses may be required if VRRP or another redundancy protocol is implemented.
- Subnet capacity must be reviewed if additional devices require IP addresses.
- Server, voice, CCTV, guest, and management VLANs have preliminary subnet allocations that require final validation.

**Status:** Preliminary addressing plan — subject to design review.

## 6. Inter-VLAN Routing and Default Gateway Design

### Proposed Routing Architecture

Northstar Business Solutions will use Layer 3 switching for internal inter-VLAN routing.

### Design Justification

The Layer 3 switching approach was selected because:

- The company requires communication between nine VLANs.
- Internal business applications may generate substantial inter-VLAN traffic.
- Layer 3 switches can perform routing using Switch Virtual Interfaces (SVIs).
- The architecture supports departmental segmentation and controlled inter-VLAN communication.
- The design should accommodate future growth in users and network devices.

### Proposed Responsibilities

| Network Component | Responsibility |
|---|---|
| Layer 3 Switch | Inter-VLAN routing and VLAN gateways |
| Layer 2 Access Switches | Endpoint connectivity |
| Edge Firewall/Router | Internet connectivity, NAT/PAT and perimeter security |
| ACLs / Security Policies | Restrict unauthorised communication between VLANs |

### Security Considerations

- Finance resources must be protected from unauthorised access.
- Guest VLAN traffic must be restricted from internal business networks.
- Network management access must be limited to authorised administrators.
- Inter-VLAN traffic will be controlled using appropriate ACLs or firewall policies.

### Gateway Redundancy

The use of two Layer 3 switches with a first-hop redundancy protocol such as VRRP will be evaluated.

The final design must consider device failures, redundant uplinks and the business availability requirements.

SVI addresses and default gateway configurations will be finalised after the redundancy architecture is selected.

**Design Status:** Layer 3 routing selected provisionally. Equipment, gateway redundancy and security policies pending validation.

## 7. Network Redundancy and High Availability

### Proposed Redundancy Architecture

Northstar Business Solutions will evaluate the use of two Layer 3 switches to provide gateway redundancy for critical network services.

### Design Justification

The redundant Layer 3 architecture is recommended because:

- The business requires core network recovery within 30 minutes.
- A single Layer 3 switch would introduce a significant single point of failure.
- Two Layer 3 switches can provide default gateway redundancy using VRRP.
- Redundancy can reduce the impact of certain hardware failures.
- The architecture supports improved availability for business-critical applications.

### Proposed Gateway Failover

| Component | Normal Operation | Primary Switch Failure |
|---|---|---|
| Layer 3 Switch 1 | VRRP Master | Unavailable |
| Layer 3 Switch 2 | VRRP Backup | Becomes VRRP Master |
| Virtual Default Gateway | Active on Switch 1 | Taken over by Switch 2 |

VRRP allows the virtual gateway IP address to remain unchanged during failover.

### Additional Availability Considerations

The network design must also evaluate:

- Redundant uplinks between access switches and Layer 3 switches.
- Suitable loop prevention or link aggregation mechanisms.
- Upstream connectivity from both Layer 3 switches.
- UPS protection for critical networking equipment.
- Failure of access switches and the edge firewall.
- Internet connectivity and possible ISP redundancy.
- Appropriate monitoring and failure detection.

### Testing and Validation

The redundancy design must be tested by simulating failures and verifying:

1. VRRP Master and Backup operation.
2. Successful gateway failover.
3. Continued connectivity through the backup switch.
4. Restoration of service after recovery.
5. Measured downtime against the 30-minute requirement.

### Budget Considerations

The proposed redundancy solution must be evaluated against the R350,000 project budget.

Final equipment selection will depend on cost, compatibility, performance and availability requirements.

**Design Status:** Two Layer 3 switches proposed. Redundancy configuration, equipment costs and failover performance pending validation.

## 8. Physical Network Topology

### Proposed Physical Architecture

Northstar Business Solutions will use a redundant switching architecture connecting access switches to two Layer 3 switches.

Both Layer 3 switches are provisionally planned for installation in the ground-floor server room.

### Access Switch Distribution

| Location | Proposed Equipment |
|---|---|
| Ground Floor | 3 Layer 2 Access Switches |
| First Floor | 2 Layer 2 Access Switches |
| Main Server Room | 2 Layer 3 Switches |
| Network Edge | Firewall/Router |

The quantities are preliminary estimates and must be validated against port capacity, PoE requirements, device distribution and budget.

### Uplink Design

Each access switch will be evaluated for two uplinks:

- Primary connection to Layer 3 Switch 1.
- Secondary connection to Layer 3 Switch 2.

This provides an alternative network path if one uplink or Layer 3 switch fails.

### Inter-Floor Connectivity

The first-floor access switches will connect to the ground-floor switching infrastructure through suitable inter-floor cabling.

Fibre-optic uplinks are proposed for evaluation.

The following must be confirmed:

- Actual inter-floor cable distances.
- Available cable pathways.
- Fibre type and transceiver compatibility.
- Required uplink speeds.
- Availability of physically diverse cable routes.

### Loop Prevention and Failover

Redundant Layer 2 connections require appropriate loop prevention and failover configuration.

Spanning Tree Protocol (STP) will be evaluated to prevent switching loops and support alternate forwarding paths.

VRRP will provide default gateway redundancy between the Layer 3 switches.

The STP topology and VRRP gateway roles must be coordinated.

### Design Limitations

Redundant uplinks do not protect against every possible failure.

Remaining risks include:

- Complete access switch failure.
- Shared power or communications room failure.
- Edge firewall/router failure.
- Failure of a shared inter-floor cable pathway.

These risks must be considered during the final high-availability design.

**Design Status:** Dual-uplink architecture proposed. Physical connectivity, equipment compatibility, failover configuration and budget pending validation.

## 9. WAN Redundancy and Internet Availability

### Proposed WAN Architecture

Northstar Business Solutions will evaluate two independent Internet Service Providers (ISPs) to reduce dependency on a single provider.

The preferred configuration is primary and secondary WAN connectivity with automatic failover.

### Design Justification

- The business has a one-hour internet recovery target.
- Two ISPs reduce dependency on a single service provider.
- Automatic failover can reduce interruption during an ISP outage.
- Critical cloud applications and communications require reliable internet access.

### Technical Requirements

- Verify underlying network and physical route diversity.
- Confirm adequate backup bandwidth.
- Configure WAN health monitoring and automatic failover.
- Ensure firewall security policies and NAT support both WAN connections.
- Test connectivity, VPN services and application recovery during failover.

### Budget Considerations

The initial R350,000 project budget must account for any required installation and network equipment costs.

Monthly internet subscriptions are funded separately.

### Validation

Simulate a primary ISP outage and verify that traffic successfully uses the secondary ISP.

Record the measured failover time and any interrupted application sessions.

**Design Status:** Two-ISP WAN redundancy proposed. Provider availability, route diversity, installation costs and failover performance pending validation.
