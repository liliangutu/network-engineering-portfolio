# OSPF Troubleshooting Scenarios

## Overview

This document describes four deliberately introduced OSPF configuration faults in a multi-area enterprise routing lab.

Each scenario follows a structured troubleshooting methodology: identify the symptoms, inspect protocol and routing state, isolate the root cause, apply a correction, and validate recovery.

## Scenario 1 — OSPF Area Mismatch

**Fault:** R2's GigabitEthernet0/1 was incorrectly assigned to Area 10 instead of backbone Area 0.

### Symptoms

- PC1 lost connectivity to remote destinations.
- The default route disappeared from R1's routing table.
- Direct IP connectivity between R2 and R3 remained available.

### Investigation

Commands used:

```text
show ip interface brief
show ip ospf neighbor
show ip ospf interface brief
show ip route
show ip protocols
```

The interface-level OSPF configuration revealed an incorrect area assignment on R2's link toward R3.

### Root Cause

The two ends of the R2–R3 transit link were assigned to different OSPF areas, preventing a valid adjacency from being maintained under normal OSPF operation.

### Resolution

On R2:

```cisco
interface GigabitEthernet0/1
 ip ospf 10 area 0
```

### Verification

The correct lab configuration restored backbone connectivity and external default-route learning.

**Packet Tracer observation:** During fault recovery, the simulator retained inconsistent OSPF neighbor and LSA information even after the interface configuration was corrected. A known-good saved lab was used to confirm expected protocol behavior.

### Lesson Learned

Physical and IP connectivity do not guarantee a functional OSPF adjacency. Verify OSPF area assignments and routing information independently.

---

## Scenario 2 — Passive-Interface Misconfiguration

**Fault:** R4's GigabitEthernet0/0 interface toward R5 was incorrectly configured as passive.

### Symptoms

- PC2 could reach its local default gateway.
- PC2 could not reach PC1 or the external server.
- R5 had no OSPF-learned remote routes.
- The R4–R5 OSPF adjacency was lost.

### Investigation

Commands used:

```text
show ip route
show ip ospf neighbor
show ip protocols
show ip ospf interface brief
```

The investigation identified the passive-interface configuration on the R4–R5 transit link.

### Root Cause

OSPF Hello packets were suppressed on R4's transit interface, preventing the routers from maintaining their neighbor relationship.

### Resolution

On R4:

```cisco
router ospf 10
 no passive-interface GigabitEthernet0/0
```

### Verification

The known-good lab confirmed the expected FULL adjacency and remote OSPF routes.

**Packet Tracer observation:** The simulator displayed inconsistent passive-interface and OSPF interface state during recovery. Interface shutdown/no shutdown did not resolve the retained state.

### Lesson Learned

Passive interfaces are appropriate for LANs without OSPF neighbors, but not for active OSPF transit links. Correlate neighbor, interface, protocol, and routing-table information rather than relying on a single command.

---

## Scenario 3 — Missing OSPF Network Advertisement

**Fault:** OSPF was removed from R5's LAN interface, GigabitEthernet0/0.

### Symptoms

- PC2 could reach its local gateway.
- Connectivity between PC1 and PC2 failed.
- R4 continued learning other OSPF routes from R5.
- The specific route to `10.20.10.0/24` disappeared from R4's routing table.

### Investigation

Commands used:

```text
tracert 10.10.10.10
show ip route
show ip route 10.20.10.10
show ip ospf
show running-config
```

The R4–R5 routing relationship remained operational, but the LAN B prefix was no longer advertised into Area 20.

### Root Cause

The LAN interface on R5 was removed from OSPF, preventing its connected subnet from being advertised.

### Resolution

On R5:

```cisco
interface GigabitEthernet0/0
 ip ospf 10 area 20
```

### Verification

The missing route returned to R4's routing table, and end-to-end connectivity between PC1 and PC2 was restored without restarting OSPF.

### Lesson Learned

An OSPF adjacency in FULL state does not guarantee that every required network is advertised. Always verify the destination prefix in the routing table.

---

## Scenario 4 — Default Route Propagation Failure

**Fault:** The `default-information originate` command was removed from R3's OSPF configuration.

### Symptoms

- Internal OSPF routing remained functional.
- PC1 could not reach the external server.
- R1 no longer had an OSPF default route.
- R3 retained its static default route toward the ISP.

### Investigation

Commands used:

```text
show ip route
show ip route 0.0.0.0
show running-config
show ip ospf database external
```

The investigation distinguished between the existence of a local static default route on R3 and its advertisement into OSPF.

### Root Cause

R3 was no longer originating the external default-route advertisement.

### Resolution

On R3:

```cisco
router ospf 10
 default-information originate
```

### Verification

R1 relearned the `O*E2` default route, and connectivity to the external server was restored.

### Lesson Learned

A default route on an ASBR is not automatically advertised to other OSPF routers. External route origination must be configured explicitly.

---

## Overall Troubleshooting Lessons

The scenarios demonstrate several important principles:

- Verify the network layer and routing protocol separately.
- Confirm OSPF area membership and neighbor adjacency state.
- Inspect the routing table even when adjacencies are FULL.
- Distinguish missing network advertisements from adjacency failures.
- Understand the relationship between ABRs, ASBRs, LSAs, and external routes.
- Validate corrective actions using both routing information and end-to-end connectivity.
- Recognize simulator limitations and avoid interpreting inconsistent Packet Tracer behavior as standard Cisco IOS operation.

## Lab Environment

Cisco Packet Tracer — Five internal Cisco routers, three OSPF areas, and a simulated external ISP.
