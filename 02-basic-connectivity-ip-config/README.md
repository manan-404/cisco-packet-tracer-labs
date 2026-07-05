# Basic Connectivity & IP Configuration

## Objective
Establish basic point-to-point and switched connectivity between end devices, 
assign static IP addresses, and verify communication using ping. Also observe 
the behavioral difference between a hub and a switch.

## Topology
- Devices used: 2x PC (direct crossover connection), PC(s) + Switch (star topology)
- Topology diagram: see `screenshots/topology-diagram.png`

## IP Addressing Scheme
| Device | Interface | IP Address     | Subnet Mask     |
|--------|-----------|-----------------|------------------|
| PC0    | Fa0       | [your real IP]  | 255.255.255.0    |
| PC1    | Fa0       | [your real IP]  | 255.255.255.0    |

## Configuration & Observations
1. Connected two PCs directly via crossover cable, assigned static IPs on same subnet.
2. Connected multiple PCs via a switch, assigned static IPs.
3. Compared hub vs switch behavior using Simulation Mode.
4. (Planned) Verify switch MAC address learning using `show mac address-table`.
   
## Verification
- `ping` between directly connected PCs → Success
- `ping` between switched PCs → Success
- Screenshot: `screenshots/ping-test.png`
- `show mac address-table` — pending, to be added once source .pkt file is accessible again

## Concepts Covered
- Static IP addressing
- Crossover vs straight-through cabling (practical use)
- Hub (broadcast/flooding) vs Switch (per-port forwarding) behavior
- Cisco Packet Tracer Simulation Mode


