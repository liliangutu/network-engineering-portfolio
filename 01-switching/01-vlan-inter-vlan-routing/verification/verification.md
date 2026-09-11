# Verification & Validation

This document contains the verification procedures used to validate the network design, Layer 2 and Layer 3 connectivity, management access, security policies, and Internet connectivity.

## 1. VLAN Verification

VLANs were verified on the core and access switches.

Commands:

```cisco
show vlan brief
```

Expected VLANs:

| VLAN | Name | Purpose |
|---|---|---|
| 10 | MANAGEMENT | Network device management |
| 20 | USERS | Corporate user devices |
| 30 | SERVERS | Internal servers |
| 40 | GUEST | Guest network |
| 99 | NATIVE | Native VLAN for trunk links |

---

## 2. Trunk Verification

802.1Q trunk links were verified between CORE1 and the access switches.

Commands:

```cisco
show interfaces trunk
```

Trunk links:

- CORE1 Fa0/1 ↔ SW1 Gi0/1
- CORE1 Fa0/2 ↔ SW2 Gi0/1

Allowed VLANs:

```text
10,20,30,40,99
```

Native VLAN:

```text
99
```

---

## 3. Layer 3 Interface Verification

The VLAN SVIs on CORE1 were verified using:

```cisco
show ip interface brief
```

Expected gateways:

| Interface | IP Address | Purpose |
|---|---|---|
| VLAN10 | 10.10.10.1/24 | Management gateway |
| VLAN20 | 10.10.20.1/24 | Users gateway |
| VLAN30 | 10.10.30.1/24 | Servers gateway |
| VLAN40 | 10.10.40.1/24 | Guest gateway |
| Gi0/1 | 10.10.254.2/30 | Routed link to R1 |

All active SVIs were verified as **up/up**.

---

## 4. Routing Verification

Routing on CORE1 was verified using:

```cisco
show ip route
```

CORE1 contains connected routes for all internal VLANs and a default route toward R1:

```text
0.0.0.0/0 → 10.10.254.1
```

R1 contains a summarized route toward the internal enterprise network:

```text
10.10.0.0/16 → 10.10.254.2
```

and a default route toward the simulated ISP:

```text
0.0.0.0/0 → 203.0.113.2
```

---

## 5. Internal Connectivity Tests

The following connectivity tests were successfully completed:

| Source | Destination | Test | Result |
|---|---|---|---|
| PC1 – 10.10.20.11 | PC3 – 10.10.20.12 | Same-VLAN ping | PASS |
| PC1 – 10.10.20.11 | 10.10.20.1 | Default gateway ping | PASS |
| PC1 – 10.10.20.11 | SERVER1 – 10.10.30.10 | Inter-VLAN ping | PASS |
| PC1 – 10.10.20.11 | PC2 – 10.10.40.11 | Inter-VLAN ping | PASS before guest ACL validation |

The tests confirm correct Layer 2 switching and Layer 3 inter-VLAN routing.

---

## 6. SSH Management Verification

SSH version 2 was configured for network-device management.

Management access is restricted using the `SSH-MANAGEMENT` ACL.

```cisco
show ssh
show access-lists
```

Validation:

| Source | Destination | Result |
|---|---|---|
| ADMIN-PC / VLAN10 | SW1 | PASS |
| ADMIN-PC / VLAN10 | SW2 | PASS |
| ADMIN-PC / VLAN10 | CORE1 | PASS |
| PC1 / VLAN20 | SW1 | DENIED |
| PC1 / VLAN20 | CORE1 | DENIED |

This confirms that SSH management access is permitted from the management network while access from the user VLAN is blocked.

---

## 7. Guest Network Isolation

An extended ACL named `GUEST-ISOLATION` was applied inbound on VLAN40.

```cisco
show access-lists GUEST-ISOLATION
```

Validation:

| Source | Destination | Result |
|---|---|---|
| GUEST | MANAGEMENT VLAN | DENIED |
| GUEST | USERS VLAN | DENIED |
| GUEST | SERVERS VLAN | DENIED |
| GUEST | VLAN40 Gateway | PASS |
| GUEST | Internet | PASS |

This provides Internet connectivity for guest clients while preventing access to internal enterprise resources.

---

## 8. NAT/PAT Verification

R1 performs Port Address Translation for the internal `10.10.0.0/16` address space.

Commands:

```cisco
show ip nat translations
show ip nat statistics
```

NAT inside:

```text
R1 GigabitEthernet0/0
```

NAT outside:

```text
R1 GigabitEthernet0/1
```

PAT uses the R1 ISP-facing address:

```text
203.0.113.1
```

---

## 9. Internet Connectivity

The simulated Internet server uses:

```text
IP address:      198.51.100.10/24
Default gateway: 198.51.100.1
```

Successful tests:

| Source | Destination | Result |
|---|---|---|
| PC1 / USERS | 198.51.100.10 | PASS |
| PC2 / GUEST | 198.51.100.10 | PASS |

These tests confirm end-to-end routing and NAT/PAT operation.

---

## Verification Summary

The completed validation confirms:

- VLAN segmentation operates correctly.
- 802.1Q trunks carry the required VLANs.
- Inter-VLAN routing is operational.
- Management interfaces are reachable through VLAN10.
- SSH management access is restricted to the management network.
- Guest clients are isolated from internal enterprise networks.
- Static routing provides connectivity between the enterprise and ISP networks.
- NAT/PAT provides external connectivity for private IPv4 hosts.
- End-to-end connectivity to the simulated Internet network is operational.

**Overall project status: PASS**
