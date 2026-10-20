# Network Engineering Portfolio

Hands-on network engineering portfolio focused on **enterprise networking, network security, troubleshooting, and automation** using Cisco technologies and practical lab environments.

This repository documents my continued development as a Network Engineer, progressing from strong routing and switching fundamentals toward advanced enterprise networking and CCNP-level technologies.

Projects are designed to demonstrate not only device configuration, but the complete engineering process:

**Design → Implementation → Security → Verification → Troubleshooting → Documentation**

---

## About This Portfolio

The projects in this repository are built around realistic network scenarios rather than isolated configuration exercises.

Each project may include:

- Network topology and design decisions
- IP addressing and VLAN architecture
- Cisco IOS configurations
- Routing and switching implementation
- Security controls and access policies
- Verification procedures
- Troubleshooting notes
- Packet Tracer or other lab files
- Automation scripts where applicable

Device configurations published in this repository are sanitized and do not contain production credentials or sensitive information.

---

## Projects

### 01 — Enterprise VLAN Segmentation & Inter-VLAN Routing

[View Project](01-switching/01-vlan-inter-vlan-routing/)

Enterprise-style network implementing segmented access networks and controlled Internet connectivity.

**Technologies:** VLANs · 802.1Q · STP/PVST · SVIs · Inter-VLAN Routing · Static Routing · ACLs · SSHv2 · NAT/PAT

Key features:

- Management, Users, Servers, and Guest VLAN segmentation
- Layer 3 switching and inter-VLAN routing
- Dedicated management network
- SSH management access restrictions
- Guest network isolation using ACLs
- Routed enterprise edge
- Static routing to a simulated ISP
- NAT/PAT Internet connectivity
- Structured verification and troubleshooting documentation

---

### 02 — Multi-Area OSPF Routing & Troubleshooting

[View Project](02-routing/01-multi-area-ospf/)

Enterprise-style multi-area OSPF network implementing hierarchical routing, inter-area summarization, and external connectivity through a simulated ISP.

**Technologies:** OSPFv2 · Multi-Area Routing · ABR · ASBR · Route Summarization · Type-3/4/5 LSAs · Default Route Origination · Routing Troubleshooting

Key features:

- Three-area OSPF architecture (Areas 0, 10, and 20)
- Five internal routers, including two ABRs and one ASBR
- OSPF neighbor adjacency establishment and verification
- Inter-area route summarization using /22 prefixes
- External default-route propagation using OSPF E2 routes
- Link-State Database analysis covering Type-3, Type-4, and Type-5 LSAs
- End-to-end connectivity between internal LANs and an external server
- Four documented troubleshooting scenarios with root-cause analysis and corrective actions
- Device configurations, verification results, topology diagram, and Cisco Packet Tracer lab

---

### 03 — EtherChannel & STP Redundancy

[View Project](01-switching/02-etherchannel-stp-redundancy/)

Enterprise-style Layer 2 switching topology implementing link aggregation, loop prevention, and redundant connectivity across distribution and access switches.

**Technologies:** Cisco IOS · LACP · EtherChannel · Rapid PVST+ · 802.1Q Trunking · Layer 2 Redundancy · Switching Troubleshooting

Key features:

- Two distribution switches and three interconnected access switches
- LACP EtherChannel using two Gigabit Ethernet links
- VLAN trunking for Users, Servers, Guest, and Management networks
- Rapid PVST+ with primary and secondary root bridges
- Spanning Tree path selection and loop prevention
- End-to-end connectivity verification across the switching topology
- Successful EtherChannel member-link failover testing
- Systematic troubleshooting of inconsistent STP behavior observed in Cisco Packet Tracer
- Device configurations, topology file, verification screenshots, and documented test results

---

## Technology Areas

The portfolio is organized around several network engineering disciplines:

### Switching

- VLAN design and segmentation
- 802.1Q trunking
- Inter-VLAN routing
- Rapid PVST+ and root bridge selection
- LACP EtherChannel
- Layer 2 redundancy and failover testing
- Spanning Tree troubleshooting

### Routing

- Inter-VLAN routing
- Static and default routing
- OSPF
- Route summarization

### Network Security

- Infrastructure management security
- ACL design
- Network segmentation
- Layer 2 security
- Device hardening
- Secure remote administration

### Troubleshooting

- Layer 2 troubleshooting
- Routing troubleshooting
- Connectivity analysis
- Protocol verification
- Failure isolation

### Network Automation

Planned automation projects will include:

- Python
- Netmiko
- REST APIs
- Ansible
- Configuration automation
- Network state validation

---
## Repository Structure

```text
network-engineering-portfolio/
│
├── 01-switching/
│   ├── 01-vlan-inter-vlan-routing/
│   └── 02-etherchannel-stp-redundancy/
│
├── 02-routing/
│   └── 01-multi-area-ospf/
│
├── 03-network-security/       # Planned
├── 04-troubleshooting/        # Planned
├── 05-network-automation/     # Planned
├── 06-real-world-projects/    # Planned
└── 07-documentation/          # Planned
```

The portfolio currently includes **three documented hands-on networking projects**, covering enterprise switching, multi-area routing, Layer 2 redundancy, and structured troubleshooting.

Additional projects focused on network security, advanced routing, automation, and enterprise infrastructure are planned.

Planned directories represent future areas of portfolio development and will be added as projects are completed.

---

## Certifications & Professional Development

- **Cisco Certified Network Associate (CCNA)**
- Continuing development toward **CCNP Enterprise**
- Focus areas: Enterprise Networking · Network Security · Troubleshooting · Automation

---

## Current Focus

My current development focus is on advancing from core enterprise networking into:

- Advanced OSPF optimization and troubleshooting
- Network resiliency and redundancy
- Route filtering and redistribution
- BGP
- Infrastructure security
- Network automation and programmability

---

## Contact

**Lilian Gutu**

Network Engineer | Network Security

- [LinkedIn](https://www.linkedin.com/in/lilian-gutu-3872b2164/)
- [GitHub](https://github.com/liliangutu)

---

> This portfolio is continuously developed as new network engineering projects, troubleshooting scenarios, and automation workflows are completed.
