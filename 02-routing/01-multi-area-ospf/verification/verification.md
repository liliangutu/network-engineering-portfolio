# OSPF Verification and Validation

## 1. OSPF Neighbor Adjacencies

**Device:** R3 (Backbone Area 0)

Command:

`show ip ospf neighbor`

```text
Neighbor ID     Pri  State       Address       Interface
4.4.4.4         1    FULL/DR     10.0.0.6      GigabitEthernet0/0
2.2.2.2         1    FULL/BDR    10.0.0.1      GigabitEthernet0/1
```

**Result:** Both backbone OSPF adjacencies are fully established.

## 2. Inter-Area Routing and Summarization

**Device:** R3

Command:

`show ip route ospf`

Relevant routes:

```text
O IA  10.10.8.0/22    via 10.0.0.1
O IA  10.20.8.0/22    via 10.0.0.6
O IA  10.10.255.0/30  via 10.0.0.1
O IA  10.20.255.0/30  via 10.0.0.6
```

**Result:** Both ABRs successfully advertise summarized inter-area prefixes. Transit networks remain advertised separately.

## 3. Type-3 Summary LSAs

**Device:** R3

Command:

`show ip ospf database summary`

| Link State ID | Advertising Router | Network Mask |
|---|---|---|
| 10.10.8.0 | 2.2.2.2 (R2) | /22 |
| 10.20.8.0 | 4.4.4.4 (R4) | /22 |
| 10.10.255.0 | 2.2.2.2 (R2) | /30 |
| 10.20.255.0 | 4.4.4.4 (R4) | /30 |

**Result:** Type-3 LSAs confirm inter-area prefix summarization by R2 and R4.

## 4. Type-4 ASBR Summary LSA

**Device:** R1 (Area 10)

Command:

`show ip ospf database asbr-summary`

Relevant entry:

```text
LS Type: Summary Links(AS Boundary Router)
Link State ID: 3.3.3.3
Advertising Router: 2.2.2.2
Metric: 1
```

**Result:** R2 advertises ASBR reachability information into Area 10.

**Simulator note:** Packet Tracer also displayed an unexpected Type-4 entry advertised by R1 itself. This was not treated as expected OSPF behavior.

## 5. Type-5 External LSA

**Device:** R1

Command:

`show ip ospf database external`

Relevant entry:

```text
LS Type: AS External Link
Link State ID: 0.0.0.0
Advertising Router: 3.3.3.3
Network Mask: /0
Metric Type: 2
Metric: 1
Forward Address: 0.0.0.0
```

**Result:** R3 originates the default route as an OSPF Type-5 AS-External LSA.

## 6. Default Route Installation

**Device:** R1

Command:

`show ip route 0.0.0.0`

Relevant output:

```text
Routing entry for 0.0.0.0/0, supernet
Known via "ospf 10", distance 110, metric 1
Tag 1, type extern 2, forward metric 1
* 10.10.255.2, from 2.2.2.2, via GigabitEthernet0/0
```

**Result:** R1 successfully installs the external Type-2 default route learned through OSPF.

## 7. End-to-End Connectivity

The following connectivity tests were performed during lab validation:

| Source | Destination | Result |
|---|---|---|
| PC1 (10.10.10.10) | PC2 (10.20.10.10) | Successful |
| PC2 (10.20.10.10) | PC1 (10.10.10.10) | Successful |
| PC1 | Internet Server (198.51.100.10) | Successful |
| PC2 | Internet Server (198.51.100.10) | Successful |

**Result:** Inter-area routing, default-route propagation, and ISP return routing provide end-to-end connectivity.

## Conclusion

The verification results demonstrate successful implementation of multi-area OSPF, ABR route summarization, ASBR default-route origination, and external connectivity.

The lab also validates the relationship between OSPF neighbor adjacencies, the link-state database, and installed routing-table entries.
