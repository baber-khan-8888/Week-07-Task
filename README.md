# Enterprise VPN, Firewall, and Secure Access Implementation using GNS3

## Overview

This project demonstrates the design and implementation of a secure multi-site enterprise network in GNS3, focused on protecting inter-site connectivity end-to-end. The topology connects a Head Office (HQ) and a remote Branch Office (BRANCH-1) through a simulated ISP router, reflecting how real WAN edges interconnect. Routing between sites is established with OSPF and secured with MD5 authentication, the link between HQ and BRANCH-1 is encrypted with a site-to-site IPsec VPN, traffic entering and leaving each site is controlled by a zone-based firewall, device management access is hardened and restricted to SSH, and centralized syslog monitoring provides visibility across the environment. The project includes configuration, verification, and testing to validate all implemented security controls.

## Features Implemented

- OSPF Dynamic Routing with MD5 Authentication
- OSPF Passive Interfaces on LAN-facing links
- Site-to-Site IPsec VPN (ISAKMP/IKE Phase 1, IPsec Phase 2, Pre-Shared Keys, Transform Sets, Crypto Maps)
- Zone-Based Policy Firewall (Security Zones, Zone-Pairs, Class-Maps, Policy-Maps, Service Policies)
- Secure Device Management (SSH-only access, disabled Telnet, password encryption, hardened console/VTY)
- Centralized Logging via Syslog
- End-to-End Network Verification Across Both LANs

## Network Topology

| Site | Router | LAN Network | Role |
|---|---|---|---|
| Simulated ISP | MAIN-ROUTER | 10.0.1.0/30, 10.0.2.0/30 | Transit / Core |
| Head Office | HQ | 192.168.10.0/24 | Site router |
| Branch Office | BRANCH-1 | 192.168.20.0/24 | Site router |

HQ and BRANCH-1 connect to each other only through MAIN-ROUTER (no direct HQ–BRANCH-1 link), with the IPsec VPN tunnel and zone-based firewall applied at each site's WAN edge.

## Devices Used and Technologies

**Devices**
- 3x Cisco c3725 Routers (MAIN-ROUTER, HQ, BRANCH-1)
- Cisco IOU Switches (SWITCH-1, SWITCH-2)
- Virtual PCs (VPCS)
- AAA Docker container (authentication services)
- Network-toolkit Docker server (syslog / monitoring services)

**Technologies**
- OSPF Multi-Router Dynamic Routing with MD5 Authentication
- Site-to-Site IPsec VPN (ISAKMP, IPsec Phase 2, ESP Transform Sets)
- Zone-Based Policy Firewall (ZBFW)
- SSHv2 and AAA-based Device Management
- Syslog Centralized Logging
- Point-to-Point WAN Links using /30 Subnets

## Testing and Verification

The project was validated using Cisco IOS verification commands and end-to-end connectivity testing.

| Test | Verification Method | Result |
|---|---|---|
| OSPF Neighbor & Authentication Verification | `show ip ospf neighbor` and `show ip ospf interface` | Full adjacencies formed on all routers with MD5 authentication enabled |
| VPN Tunnel Verification | `show crypto isakmp sa` and `show crypto ipsec sa` | ISAKMP SA in QM_IDLE/ACTIVE state; active ESP encaps/decaps between HQ and BRANCH-1 |
| Firewall Policy Verification | `show zone security`, `show zone-pair security`, `show policy-map type inspect zone-pair sessions` | Zone-pairs and inspect policies active and matching traffic on both HQ and BRANCH-1 |
| Secure Access Verification | Telnet and SSH connection attempts | Telnet refused (disabled); SSH successfully reached the router login prompt |
| End-to-End Connectivity | ICMP Ping between HQ and BRANCH-1 LANs | Successful bidirectional communication across the encrypted, firewalled path |

The verification results confirm that OSPF routing security, the IPsec VPN, the zone-based firewall, and secure device management all function together as an integrated, defense-in-depth design.

## Repository Contents

| Item | Description |
|---|---|
| GNS3 Project Folder | Complete GNS3 project containing the entire topology and device configurations. |
| Screenshots/ | Screenshots demonstrating the topology, OSPF/VPN/firewall verification commands, secure access testing, and successful connectivity tests. |
| Taskreport.pdf | Detailed project report documenting the design, IP addressing, VPN and firewall configuration, secure access hardening, monitoring setup, testing procedures, and verification results. |

## Author

**Baber Khan**
BS Telecommunication Engineering
