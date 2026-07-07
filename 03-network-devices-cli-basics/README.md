# Network Devices & CLI Basics

## Objective
Build a multi-device network (PCs, laptops, hub, switch, router), assign IPs, 
configure basic CLI settings (hostname, MOTD), and understand core device 
distinctions at different OSI layers.

## Topology
- 3x PC connected to a Switch
- 3x Laptop connected to a Hub
- Switch and Hub both connect to a central Router
- Screenshots: `screenshots/01-devices-placed.png`, `screenshots/02-topology-connected.png`

## Configuration Steps
1. Assigned IP addresses via CLI on router interfaces — `screenshots/03-ip-assignment-cli.png`
2. Verified connectivity across all hosts via ping — `screenshots/04-ping-all-hosts.png`
3. Reviewed device/link status using Packet Tracer's Inspect tool — `screenshots/05-device-link-status-list.png`
4. Reviewed CLI command history — `screenshots/06-cli-commands-list.png`
5. Changed switch hostname: Switch(config)#hostname Manan
Screenshot: `screenshots/07-hostname-change.png`
6. Configured MOTD banner: Router(config)#banner motd #this is the message of the day#
Screenshot: `screenshots/08-motd-banner.png`

## Theory: Device Comparisons
**Repeater vs Hub:** A repeater regenerates a signal over a single link to extend 
distance. A hub is effectively a multi-port repeater — it regenerates and floods 
the signal out to all connected ports simultaneously.

**Hub vs Switch:** A hub operates at Layer 1 and floods traffic to every port with 
no awareness of MAC addresses. A switch operates at Layer 2, learns MAC addresses 
per port, and forwards traffic only to the intended destination port.

**Router vs Switch:** A switch forwards traffic within a single network/broadcast 
domain using MAC addresses (Layer 2). A router forwards traffic between different 
networks using IP addresses (Layer 3), and defines separate broadcast domains.

**Switch vs Bridge:** A bridge connects two network segments and filters traffic 
by MAC address, typically with fewer ports. A switch is functionally a multi-port 
bridge, offering the same MAC-filtering behavior at greater scale, usually with 
dedicated hardware (ASICs) for faster forwarding.

## Concepts Covered
- CLI-based IP configuration
- Hostname and MOTD banner configuration
- Packet Tracer Inspect tool (device/link status overview)
- CLI command history review
- OSI Layer 1/2/3 device distinctions (repeater, hub, bridge, switch, router)
