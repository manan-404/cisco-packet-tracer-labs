# Dynamic Routing — OSPF

## Objective
Configure a 4-router linear topology with OSPF as the dynamic routing protocol, 
and verify end-to-end connectivity across all router LANs.

## Topology
Linear chain of 4 routers, each with its own LAN (Switch + PC) and 
serial links connecting neighbors:
R0 (LAN 192.168.10.0/24) --192.168.20.0/24-- R1 (LAN 192.168.30.0/24)
|
192.168.40.0/24
|
R3 (LAN 192.168.70.0/24) --192.168.60.0/24-- R2 (LAN 192.168.50.0/24)
Screenshots: `screenshots/01-topology-no-connections.png`, `screenshots/02-topology-with-ips.png`

## Interface IP Configuration
**Router 0:**
- Gig0/0/0: `192.168.10.1/24`
- Serial0/1/0: `192.168.20.1/24`

**Router 1:**
- Gig0/0/0: `192.168.30.1/24`
- Serial0/1/0: `192.168.20.2/24`
- Serial0/1/1: `192.168.40.1/24`

**Router 2:**
- Gig0/0/0: `192.168.50.1/24`
- Serial0/1/0: `192.168.40.2/24`
- Serial0/1/1: `192.168.60.1/24`

**Router 3:**
- Gig0/0/0: `192.168.70.1/24`
- Serial0/1/0: `192.168.60.2/24`

Screenshots: `screenshots/03-router0-config.png`, `screenshots/04-router1-config.png`, 
`screenshots/05-router2-config.png`, `screenshots/06-router3-config.png`

## OSPF Configuration
router ospf 1
network <interface-network> <wildcard-mask> area 0
applied on all 4 routers so every directly connected network is advertised 
into a single OSPF area.

## Verification — Cross-Router Connectivity (from PC0, 192.168.10.x)
C:>ping 192.168.30.2   (Router1's LAN)
Request timed out.
Reply from 192.168.30.2: bytes=32 time=2ms TTL=126
... 3/4 received (25% loss)
C:>ping 192.168.50.2   (Router2's LAN)
Request timed out.
Reply from 192.168.50.2: bytes=32 time=3ms TTL=125
... 3/4 received (25% loss)
C:>ping 192.168.70.2   (Router3's LAN)
Request timed out.
Reply from 192.168.70.2: bytes=32 time=11ms TTL=124
... 3/4 received (25% loss)
Screenshot: `screenshots/07-pc0-ping-verification.png`

**Observations:**
- Successful connectivity to LANs 2 and 3 hops away — proof OSPF correctly 
  learned and propagated routes across the entire chain, not just directly 
  connected segments.
- First ping to each destination always times out — expected ARP resolution 
  delay on first contact, not a routing failure (subsequent pings succeed).
- TTL decreases with hop count (126 → 1 hop, 125 → 2 hops, 124 → 3 hops), 
  independently confirming the actual path length matches the topology.

## Concepts Covered
- OSPF single-area configuration across a 4-router chain topology
- GUI-based routing configuration in Packet Tracer (vs CLI)
- Multi-hop connectivity verification using ping + TTL analysis
- Real-world ARP delay behavior on first packet to a new destination

## Note
Course manual also included RIP (class activity) and EIGRP (exercise) on 
separate 3-router setups — only the OSPF implementation was completed and is 
documented here.
Screenshot: `screenshots/07-pc0-ping-verification.png`

**Observations:**
- Successful connectivity to LANs 2 and 3 hops away — proof OSPF correctly 
  learned and propagated routes across the entire chain, not just directly 
  connected segments.
- First ping to each destination always times out — expected ARP resolution 
  delay on first contact, not a routing failure (subsequent pings succeed).
- TTL decreases with hop count (126 → 1 hop, 125 → 2 hops, 124 → 3 hops), 
  independently confirming the actual path length matches the topology.

## Concepts Covered
- OSPF single-area configuration across a 4-router chain topology
- GUI-based routing configuration in Packet Tracer (vs CLI)
- Multi-hop connectivity verification using ping + TTL analysis
- Real-world ARP delay behavior on first packet to a new destination

## Note
Course manual also included RIP (class activity) and EIGRP (exercise) on 
separate 3-router setups — only the OSPF implementation was completed and is 
documented here.

OSPF was configured via Packet Tracer's router GUI (Config tab → ROUTING → OSPF), 
adding network statements per interface rather than raw CLI — functionally 
equivalent to:
