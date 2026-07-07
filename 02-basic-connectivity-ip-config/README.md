# Basic Connectivity & IP Configuration

## Objective
Establish point-to-point and switched connectivity between end devices, assign 
static IP addresses, verify communication via ping, and compare hub vs switch 
behavior at the packet level using Simulation Mode.

## Activity 1: Direct (Crossover) Connectivity
### Topology
- PC0 (192.168.1.2) <--crossover--> Laptop (192.168.1.3)
- Screenshot: `screenshots/02-topology-crossover-pcs.png`

### IP Configuration
- Screenshots: `screenshots/03-ip-config-pc0.png`, `screenshots/04-ip-config-laptop.png`

### Verification
- `ping 192.168.1.3` from PC0 → Success — `screenshots/05-ping-result.png`
- Ping output breakdown: **Reply from** (responding device IP), **Bytes** (packet size), 
  **Time** (round-trip time), **TTL** (Time to Live — max hops before packet is discarded)
- Simulation Mode packet trace: `screenshots/06-simulation-mode-transfer.png`

## Activity 2: Switched Network + Hub vs Switch Comparison
### Topology
- 3x PC (192.168.1.4, .5, .6) connected via Switch
- Screenshots: `screenshots/07-topology-3pc-switch.png`, `screenshots/08-place-3pc-switch.png`

### IP Configuration & Verification
- Screenshot: `screenshots/09-ip-config-pc3.png`
- Ping validated via CLI across all hosts — `screenshots/10-ping-cli-validate.png`

### Hub vs Switch Behavior
- Same 3 PCs reconnected via Hub instead of Switch — `screenshots/11-hub-vs-switch-topology.png`
- **Observation:** A hub floods incoming traffic out of every port regardless of 
  destination, so all connected devices receive every packet. A switch forwards 
  traffic only out the port associated with the destination MAC address, reducing 
  unnecessary traffic on the network.
- Simulation Mode parameters explained: **Time** (simulation clock), **Last Device** 
  (previous hop), **At Device** (current device processing packet), **Type** (protocol/packet type)
- Screenshot: `screenshots/12-simulation-mode-parameters.png`

## Concepts Covered
- Static IP addressing (crossover and switched topologies)
- Ping diagnostics and output interpretation
- Hub (flooding) vs Switch (MAC-based forwarding) — demonstrated, not just theorized
- Cisco Packet Tracer Simulation Mode mechanics

## Note
`show mac address-table` verification is planned — pending access to the source 
.pkt file (currently on an inaccessible drive). Will be added as a follow-up commit.
