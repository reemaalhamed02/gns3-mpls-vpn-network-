# gns3-mpls-vpn-network-
Enterprise MPLS/VPN network simulation using GNS3 | BGP, OSPF, IPsec, VRF | Northumbria University
# 🌐 Enterprise MPLS/VPN Network Design – GNS3 Simulation

> **Module:** KV6017 – Enterprise Networks and Security  
> **Institution:** Northumbria University, Newcastle  
> **Designer:** Reema Abdulaziz Alhamed  

---

## 📋 Project Overview

A full enterprise-grade network simulation built in **GNS3**, implementing a multi-site MPLS Layer 3 VPN backbone connecting three regional offices: **Manchester (HQ)**, **Leeds**, and **Newcastle**.

The design combines **MPLS L3 VPN**, **IPsec encryption**, **MP-BGP**, **OSPF**, and **VRF isolation** to deliver scalable, secure, and segmented connectivity across departments.

---

## Technologies Used

| Technology | Purpose |
|-----------|---------|
| **GNS3** | Network simulation platform |
| **Cisco IOS** | Router configuration |
| **MPLS L3 VPN** | Scalable site-to-site connectivity |
| **MP-BGP (VPNv4)** | VPN route distribution |
| **OSPF Area 0** | IGP for the MPLS core |
| **LDP** | Label distribution |
| **VRF** | Customer traffic isolation |
| **IPsec (AES + SHA + IKE)** | Encryption for sensitive traffic |
| **VLSM** | Efficient IP addressing |
| **DHCP** | Dynamic client addressing |
| **Extended ACLs** | Security policies |

---

##  Requirements Implemented

### Requirement 1 – IPsec VPN (EXEC ↔ HR)

Site-to-site IPsec VPN between EXEC and HR routers at the HQ, securing sensitive inter-departmental traffic.

- **IKE Phase 1:** AES encryption + SHA hash + Pre-Shared Key auth
- **IKE Phase 2:** ESP-AES + ESP-SHA-HMAC

### Requirement 2 – MPLS L3 VPN Full Mesh (CUSTOMER_A)

Any-to-any connectivity for HR, PRODUCTION1, and LOGISTICS departments.

- **VRF:** `CUSTOMER_A`
- **RDs:** `65000:100` (MAN_HO), `65000:200` (LDS), `65000:300` (NCL)
- **RT import/export:** `65000:100`
- **MP-BGP** for VPNv4 route distribution

### Requirement 3 – Hybrid MPLS + IPsec (EXEC ↔ R&D LAN A)

Combined MPLS L3 VPN with selective IPsec on sensitive traffic only.

- **VRF:** `CUSTOMER_B` (RD `65000:400` / `65000:500`, RT `65000:200`)
- IPsec applied **only** to EXEC LAN ↔ R&D LAN A
- R&D LAN B intentionally excluded to avoid unnecessary cryptographic overhead

### Requirement 4 – SQL Server Placement (Performance Modelling)

Performance analysis to determine optimal SQL server placement:

- MAN_HO ↔ LDS: 70 Mbps / 80 km → ~11.4 sec for 100 MB query
- MAN_HO ↔ NCL: 100 Mbps / 220 km → ~8 sec for 100 MB query
- **Decision:** SQL server placed in **NCL Logistics LAN**

### 🚫 Requirement 5 – Selective Access Control

Extended ACL on NCL PE router blocks PRODUCTION2 → EXEC LAN, while permitting all other access.

### Requirement 6 – VLSM Addressing + DHCP

Variable Length Subnet Masking applied to optimize IPv4 allocation per LAN. Distributed DHCP on each CE router for reliability and isolation.

---

## Addressing Scheme

| LAN | Network | Mask | Gateway | DHCP Range |
|-----|---------|------|---------|------------|
| EXEC LAN | 10.1.1.0 | /26 | 10.1.1.1 | .10–.62 |
| HR LAN | 10.1.20.0 | /24 | 10.1.20.1 | .10–.254 |
| R&D LAN A | 10.3.1.0 | /26 | 10.3.1.1 | .10–.62 |
| R&D LAN B | 10.3.2.0 | /28 | 10.3.2.1 | .10–.14 |
| LOGISTICS LAN | 10.3.5.0 | /25 | 10.3.5.1 | .10–.126 |
| PRODUCTION2 LAN | 10.3.3.0 | /23 | 10.3.3.1 | .10–.254 |
| PRODUCTION1 LAN | 10.2.1.0 | /25 | 10.2.1.1 | .10–.126 |

---

## Implementation Phases

1. **Phase 1** – Base infrastructure & IP addressing
2. **Phase 2** – MPLS core (OSPF + LDP)
3. **Phase 3** – VRF + MP-BGP (VPNv4)
4. **Phase 4** – IPsec encryption for sensitive traffic
5. **Phase 5** – Access control policies (ACLs)
6. **Phase 6** – DHCP + end-to-end testing

A bottom-up approach was used to isolate troubleshooting at each layer.

---

## Troubleshooting Strategy

OSI-model-based methodology — bottom up:

| Layer | Common Issues | Verification |
|-------|--------------|--------------|
| Physical / Data Link | Interface status, cabling | `show interface` |
| Routing (OSPF) | Adjacency states, MTU mismatches | `show ip ospf neighbor` |
| MPLS / LDP | Label distribution, CEF | `show mpls ldp neighbor` |
| MP-BGP / VRF | Route exchange, RT mismatches | `show bgp vpnv4 unicast all` |
| IPsec | Phase 1 vs Phase 2 failures | `show crypto isakmp sa` |
| Security (ACLs) | Direction, rule order | ACL hit counters |

---

## Skills Demonstrated

- `GNS3` · `Cisco IOS` · `Enterprise Network Design`
- `MPLS L3 VPN` · `MP-BGP` · `OSPF` · `LDP` · `VRF`
- `IPsec (IKE / AES / SHA / HMAC)` · `Site-to-Site VPN`
- `VLSM Subnetting` · `DHCP` · `Extended ACLs`
- `Performance Modelling` · `Network Troubleshooting`

---

## 📁 Repository Contents
