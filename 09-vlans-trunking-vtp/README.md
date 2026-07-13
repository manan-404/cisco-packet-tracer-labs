# VLANs, Trunking & VTP

## Objective
Segment a switched network into multiple VLANs, configure router-on-a-stick 
for inter-VLAN routing, implement VTP to synchronize VLAN info across 
multiple switches, and verify connectivity within/across VLANs.

## Topology
- Multiple switches connected via trunk links, router providing inter-VLAN 
  routing via sub-interfaces, 4 PCs distributed across VLANs
- Screenshot: `screenshots/01-topology.png`

## Router Configuration (Router-on-a-Stick)
- Sub-interfaces configured per VLAN for inter-VLAN routing
- Screenshots: `screenshots/02-router-config.png`, `screenshots/03-router-subinterfaces.png`

## Switch Configuration — VLANs & Trunking
- VLANs created and assigned to access ports; trunk ports configured to carry 
  multiple VLANs between switches
- Screenshots: `screenshots/04-switch-vlan-config.png`, `screenshots/05-switch-trunk-config.png`
- Same VLAN/trunk process repeated on the other 2 switches — 
  `screenshots/06-switch2-config.png`, `screenshots/07-switch3-config.png`


## VTP (VLAN Trunking Protocol) Implementation
**Server switch:**

Switch(config)# vtp domain mynetwork

Switch(config)# vtp mode server

Switch(config)# vtp password cisco

**Client switches:**

Switch(config)# vtp domain mynetwork

Switch(config)# vtp mode client

Switch(config)# vtp password cisco

**VLANs created on server (auto-propagated to clients):**

Switch(config)# vlan 10

Switch(config-vlan)# name Sales

Switch(config)# vlan 20

Switch(config-vlan)# name HR


## Verification
Ping tested from all 4 PCs to confirm intra-VLAN and inter-VLAN connectivity:
- `screenshots/08-ping-from-pc0.png`
- `screenshots/09-ping-from-pc1.png`
- `screenshots/10-ping-from-pc2.png`
- `screenshots/11-ping-from-pc3.png`

## Theory

### Types of VLANs
- **Default VLAN:** VLAN 1, all ports belong here by default
- **Data VLAN:** carries user-generated traffic (also called "user VLAN")
- **Voice VLAN:** prioritizes IP phone traffic using QoS
- **Management VLAN:** isolated VLAN for remote device management (SSH/Telnet/SNMP)
- **Native VLAN:** carries untagged traffic on a trunk port (default = VLAN 1, 
  should be changed for security)
- **Guest VLAN:** limited-access VLAN for unauthenticated/unassigned devices

### VLAN Tagging
- **IEEE 802.1Q (Dot1Q):** industry-standard, adds a 4-byte VLAN ID tag 
  (1–4094) to the Ethernet frame, allows multiple VLANs over one trunk link
- **ISL (Inter-Switch Link):** Cisco-proprietary, encapsulates the entire 
  frame with a new header — legacy, largely replaced by 802.1Q

### VTP Modes
- **Server:** can create/modify/delete VLANs, advertises changes to other switches
- **Client:** receives VLAN info, cannot create/delete VLANs locally
- **Transparent:** forwards VTP messages without applying them; local VLANs only

### VLAN vs Subnet
| Feature | VLAN | Subnet |
|---------|------|--------|
| Definition | Logical separation at Layer 2 | Logical division at Layer 3 |
| Basis of separation | Switch port configuration | IP address range |
| OSI Layer | Layer 2 (Switching) | Layer 3 (Routing) |
| Purpose | Separate broadcast domains within a LAN | Organize IPs, control routing |
| Device used | Switches | Routers |
| Interconnection | Requires router/L3 switch (inter-VLAN routing) | Requires routers |

**Example:** VLAN 10 (Students) → subnet `192.168.10.0/24`, VLAN 20 (Teachers) 
→ subnet `192.168.20.0/24`

## Concepts Covered
- VLAN creation and port assignment across multiple switches
- 802.1Q trunking between switches
- Router-on-a-stick inter-VLAN routing
- VTP server/client synchronization with domain + password authentication
- VLAN vs Subnet distinction (Layer 2 vs Layer 3 segmentation)
