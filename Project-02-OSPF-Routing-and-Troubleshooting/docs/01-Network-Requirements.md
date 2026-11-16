# Project 02 — Network Requirements

**Project:** OSPF Routing, Connectivity & Incident Troubleshooting  
**Organisation:** NorthLink Networks (Fictional)  
**Environment:** Cisco Packet Tracer  
**Target Role:** Junior Network Engineer  
**Specialisation:** Service Provider Networking

---

## 1. Business Scenario

NorthLink Networks is a fictional Internet Service Provider operating three regional Points of Presence (PoPs).

The company requires reliable internal IP routing between its three PoPs to support network connectivity and maintain operational resilience.

As a Junior Network Engineer, the objective is to implement and verify OSPF routing, investigate simulated connectivity incidents, perform controlled recovery procedures, and document the findings.

## 2. Business Requirements

| ID | Requirement | Description |
|---|---|---|
| BR01 | Network Connectivity | Establish IP connectivity between the three PoPs. |
| BR02 | Dynamic Routing | Enable automatic route learning between routers. |
| BR03 | Redundancy | Provide an alternative routing path when a single inter-router link fails. |
| BR04 | Fault Investigation | Identify connectivity problems using Cisco IOS verification commands. |
| BR05 | Operational Recovery | Restore failed connections and verify network operation. |
| BR06 | Documentation | Maintain clear implementation and troubleshooting records. |

## 3. Technical Requirements

| ID | Requirement | Implementation |
|---|---|---|
| TR01 | Network Topology | Three Cisco 2911 routers connected in a triangular topology. |
| TR02 | IP Addressing | IPv4 /30 subnets for the three point-to-point links. |
| TR03 | Routing Protocol | OSPFv2 configured in Area 0. |
| TR04 | Router Identification | Unique OSPF router IDs for R1, R2 and R3. |
| TR05 | Route Verification | Verify OSPF neighbour relationships and routing-table entries. |
| TR06 | Connectivity Testing | Perform ICMP tests between router interfaces. |
| TR07 | Failure Testing | Simulate controlled network faults and investigate their effects. |
| TR08 | Recovery Verification | Confirm interface, routing and connectivity recovery. |

## 4. Project Scope

### In Scope

- IPv4 subnet planning
- Cisco router interface configuration
- Single-area OSPFv2
- OSPF neighbour verification
- Routing-table analysis
- Equal-Cost Multi-Path (ECMP) route verification
- Controlled fault simulation
- Network troubleshooting and recovery
- Technical documentation and evidence collection

### Out of Scope

- BGP
- MPLS and VRFs
- IPv6 routing implementation
- Internet connectivity
- Customer-facing production services
- Network monitoring platform deployment
- Application-level high availability
- Quantitative OSPF convergence-time measurement

## 5. Success Criteria

The project will be considered successful when:

1. All three router-to-router links have correct IP addressing and basic connectivity.
2. OSPF neighbour adjacencies reach the FULL state.
3. Routers learn the expected remote networks through OSPF.
4. Alternative routes remain available during the tested single-link failure.
5. Connectivity tests validate the expected routing behaviour.
6. Failed links are restored and post-recovery checks pass.
7. Simulated incidents are documented with technical evidence.

## 6. Alignment with Job Requirements

This project was designed around recurring skills identified in Junior Network Engineer and entry-level network operations job advertisements, including roles at NTT DATA, NAPAfrica, iConnect and Clickatell.

The lab focuses on:

- TCP/IP and routing fundamentals
- Cisco router configuration
- OSPF routing and troubleshooting
- Network fault identification
- Incident investigation and recovery
- Pre-change and post-change verification
- Technical documentation

The project demonstrates practical networking fundamentals but does not represent a production-ready ISP backbone.