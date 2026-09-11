# Troubleshooting Notes

This document records troubleshooting performed during the implementation and validation of the network.

## Issue 1 — Native VLAN Mismatch During Trunk Configuration

### Observation

While configuring the trunk between CORE1 and SW2, a native VLAN mismatch message was temporarily reported.

The trunk design uses:

```text
Native VLAN: 99
Allowed VLANs: 10,20,30,40,99
```

### Investigation

The trunk configuration was checked on both sides of the link.

```cisco
show interfaces trunk
```

The mismatch occurred while the trunk configuration was being applied sequentially: one side had already been changed to native VLAN 99 while the other side still had its previous/default configuration.

### Resolution

Native VLAN 99 was configured consistently on both ends of the trunk.

Final configuration:

```cisco
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,30,40,99
switchport mode trunk
```

### Result

Once both ends matched, the native VLAN mismatch condition was resolved.

---

## Issue 2 — Unexpected STP/Trunk Verification Output

### Observation

During trunk verification on SW1, the following command initially produced an unexpected result:

```cisco
show interfaces trunk
```

The output indicated:

```text
Vlans in spanning tree forwarding state and not pruned:
none
```

This appeared inconsistent with the expected operational trunk.

### Investigation

Instead of modifying the configuration immediately, additional verification was performed.

STP state for VLAN20:

```cisco
show spanning-tree vlan 20
```

The relevant ports were shown in the forwarding state:

```text
Fa0/1   Desg FWD
Gi0/1   Desg FWD
```

The trunk interface was then inspected:

```cisco
show interfaces gi0/1 switchport
```

The interface showed:

- Operational mode: trunk
- Trunk encapsulation: 802.1Q
- Native VLAN: 99
- Allowed VLANs: 10,20,30,40,99

End-to-end connectivity tests were also successful.

### Root Cause

The earlier `none` output was consistent with a temporary spanning-tree convergence state following the trunk configuration change rather than a persistent trunk failure.

### Result

No unnecessary configuration changes were made.

After convergence, the trunk operated correctly and traffic passed successfully across the switching infrastructure.

---

## Issue 3 — Unexpected STP Cost on GigabitEthernet Interface

### Observation

During STP verification, SW1 GigabitEthernet0/1 displayed an STP path cost of:

```text
19
```

At first glance, this appeared unusual for a GigabitEthernet interface.

### Investigation

The physical topology was reviewed.

SW1 Gi0/1 was connected to a FastEthernet interface on CORE1.

Ethernet links negotiate to the highest speed supported by both sides of the connection.

Therefore, although the SW1 interface is physically a GigabitEthernet port, the operational link speed is limited by the FastEthernet interface on CORE1.

### Explanation

The effective link operates at 100 Mbps.

An STP cost of 19 is therefore consistent with the operational Fast Ethernet speed used by classic STP cost values.

### Result

The STP cost was determined to be expected behavior and not a configuration fault.

---

# Troubleshooting Summary

The troubleshooting process demonstrated several important operational practices:

- Verify both ends of a trunk before changing configuration.
- Distinguish temporary protocol convergence from persistent faults.
- Use STP state to validate Layer 2 forwarding.
- Verify operational parameters rather than relying only on interface names.
- Confirm findings using end-to-end connectivity tests.
- Avoid unnecessary configuration changes when verification shows the network is operating correctly.

The network remained operational after verification, and no corrective configuration was required for the STP observation.
