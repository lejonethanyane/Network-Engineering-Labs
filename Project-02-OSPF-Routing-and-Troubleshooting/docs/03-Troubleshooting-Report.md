# Project 02 — OSPF Troubleshooting Report

**Organisation:** NorthLink Networks (Fictional)  
**Environment:** Cisco Packet Tracer  
**Network:** Three-router OSPF Area 0 backbone  
**Purpose:** Document simulated network incidents, troubleshooting procedures, corrective actions and verification results.

---

## Incident 01 — OSPF Link Failure and Recovery

**Incident ID:** INC-001  
**Status:** Resolved  
**Incident Type:** Simulated router link failure  
**Affected Connection:** R1 G0/0 ↔ R2 G0/0  
**Affected Network:** 10.0.12.0/30

### 1. Incident Description

A controlled failure was introduced by administratively disabling R1's GigabitEthernet0/0 interface, which connects R1 to R2.

The objective was to verify whether OSPF could maintain reachability to the 10.0.23.0/30 network through the alternative path via R3.

### 2. Baseline Verification

Before introducing the failure:

- All three router-to-router links were operational.
- OSPF Area 0 was configured on R1, R2 and R3.
- R1 had established FULL OSPF adjacencies with R2 and R3.
- R1 had two equal-cost OSPF routes to 10.0.23.0/30.

The routing table showed next hops of 10.0.12.2 and 10.0.13.2, both with an OSPF cost of 2.

### 3. Fault Simulation

The following commands were executed on R1:

```cisco
configure terminal
interface GigabitEthernet0/0
shutdown
end
```

The interface changed to an administratively down/down state.

### 4. Investigation and Findings

The following commands were used:

```cisco
show ip interface brief
show ip ospf neighbor
show ip route
```

The investigation established that:

1. R1's G0/0 interface was administratively disabled.
2. R1 lost its OSPF adjacency with R2.
3. The OSPF adjacency between R1 and R3 remained in the FULL state.
4. R1 removed the next hop through 10.0.12.2.
5. R1 retained the OSPF route to 10.0.23.0/30 via 10.0.13.2.

### 5. Connectivity Verification During Failure

The following tests were performed from R1:

```cisco
ping 10.0.23.2
ping 10.0.23.1
```

Both tests returned a 100% success rate (5/5).

This demonstrated reachability to R3 and R2 through the surviving network while the direct R1–R2 connection was disabled.

### 6. Recovery Procedure

The failed interface was restored on R1:

```cisco
configure terminal
interface GigabitEthernet0/0
no shutdown
end
```

### 7. Post-Recovery Verification

The following checks were completed:

- R1 G0/0 returned to up/up.
- R1 re-established a FULL OSPF adjacency with R2.
- The R1–R3 OSPF adjacency remained FULL.
- Both equal-cost OSPF next hops to 10.0.23.0/30 were restored.
- A final ping from R1 to 10.0.23.1 succeeded (5/5).

### 8. Resolution

**INC-001 was resolved successfully.**

The simulated link failure was detected, an alternative OSPF route remained available, and the original routing state was restored following recovery.

### 9. Limitations

- OSPF convergence time was not measured.
- Continuous application or customer traffic was not simulated.
- The tests confirmed ICMP connectivity and routing-table behaviour, not application-level service continuity.
- ECMP forwarding distribution was not measured.

### 10. Lessons Learned

- OSPF can maintain alternative routes when a network link becomes unavailable.
- FULL neighbour adjacencies should be verified separately from routing-table entries.
- Routing-table verification does not replace end-to-end connectivity testing.
- Recovery procedures should include interface, neighbour, route and connectivity checks.

---

## Incident 02 — OSPF Area Mismatch Troubleshooting

**Incident ID:** INC-002  
**Status:** Resolved  
**Incident Type:** Simulated OSPF configuration fault  
**Affected Connection:** R1 G0/0 ↔ R2 G0/0  
**Affected Network:** 10.0.12.0/30

### 1. Incident Description

A controlled OSPF configuration fault was introduced on R2's GigabitEthernet0/0 interface by changing its area assignment from Area 0 to Area 1.

The objective was to investigate why the OSPF neighbour relationship between R1 and R2 failed despite the physical interface remaining operational.

### 2. Baseline Verification

Before introducing the fault:

- R1 and R2 had an established FULL OSPF adjacency.
- R3 maintained FULL adjacencies with both routers.
- All three point-to-point Ethernet links were operational.
- The network was configured for single-area OSPFv2 Area 0.

### 3. Fault Simulation

The following command was applied to R2 G0/0:

```cisco
interface GigabitEthernet0/0
 ip ospf 1 area 1
```

This created an OSPF area mismatch between R1 and R2.

### 4. Investigation and Findings

The following commands were used:

```cisco
show ip ospf interface g0/0
show ip ospf neighbor
show ip route
```

The investigation established that:

1. R2 G0/0 remained up/up.
2. R1 G0/0 was configured in OSPF Area 0.
3. R2 G0/0 was configured in OSPF Area 1.
4. R2 reported zero adjacent neighbours on G0/0.
5. Cisco IOS displayed an OSPF area mismatch error.

The error confirmed that OSPF packets were being rejected because of incompatible area assignments.

### 5. Root Cause

The OSPF area assignment on R2 G0/0 did not match R1's configuration.

The approved network design required both interfaces to participate in Area 0.

### 6. Corrective Action

The following correction was applied on R2:

```cisco
configure terminal
interface GigabitEthernet0/0
 ip ospf 1 area 0
end
```

### 7. Post-Recovery Verification

The following results were observed:

- R2 G0/0 returned to OSPF Area 0.
- The R1–R2 OSPF adjacency returned to FULL.
- R2 maintained its FULL adjacency with R3.
- R1's routing table again contained two equal-cost routes to 10.0.23.0/30.
- A ping from R1 to 10.0.23.1 succeeded with 5/5 replies.

### 8. Resolution

**INC-002 was resolved successfully.**

The OSPF area mismatch was corrected, neighbour relationships recovered and IP connectivity was verified.

### 9. Additional Observation — Hello Timer Experiment

Before introducing the area mismatch, an OSPF Hello interval of 20 seconds was configured on R2 G0/0 while R1 remained at 10 seconds.

The mismatch was confirmed through interface verification commands. However, both routers continued reporting FULL adjacencies in Packet Tracer.

The Hello interval was restored to 10 seconds before the area mismatch test.

**No adjacency failure was demonstrated during the Hello timer experiment.** The result was treated as a simulation limitation or inconclusive observation, rather than a successfully reproduced outage.

### 10. Lessons Learned

- Physical interface status alone does not confirm routing protocol health.
- OSPF area assignments must be compatible between neighbours.
- Cisco IOS diagnostic messages can provide direct evidence of configuration faults.
- Corrective changes should be followed by neighbour, route and connectivity verification.
- Lab documentation should distinguish observed behaviour from expected behaviour.


## Incident 03 — OSPF Cost and Route Selection Troubleshooting

**Incident ID:** INC-003

**Status:** Resolved

**Incident Type:** Controlled OSPF metric change

**Affected Router:** R1

**Affected Interface:** GigabitEthernet0/0

**Destination Network:** 10.0.23.0/30
