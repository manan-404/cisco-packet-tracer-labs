# Network Devices & CLI Basics

## Objective
Configure a multi-device network via router/switch CLI, understand basic device 
identification commands, and distinguish between core networking devices at 
different OSI layers.

## Topology
- 3x PC connected to a Switch
- 3x Laptop connected to a Hub
- Switch and Hub both connected to a central Router
- Topology diagram: see `screenshots/topology-diagram.png`

## Configuration Steps
1. Assigned IP addresses via CLI on router/switch interfaces.
2. Set switch hostname: Switch(config)#hostname Manan
3. Configured a Message of the Day (MOTD) banner: 
Router(config)#banner motd #this is the message of the day#

## Verification
- Hostname change reflected in CLI prompt (`Manan(config)#...`) — screenshot: `screenshots/hostname-config.png`
- MOTD banner displayed on login — screenshot: `screenshots/motd-banner.png`

## Theory: Network Device Comparison
| Device    | OSI Layer | Function |
|-----------|-----------|----------|
| Repeater  | Layer 1   | Regenerates/amplifies signal, no addressing awareness |
| Hub       | Layer 1   | Multi-port repeater, floods all traffic to all ports |
| Bridge    | Layer 2   | Filters traffic by MAC address, connects two segments |
| Switch    | Layer 2   | Multi-port bridge, per-port MAC-based forwarding |
| Router    | Layer 3   | Routes traffic between networks using IP addresses |

## Concepts Covered
- CLI-based device configuration (vs GUI)
- Hostname and MOTD banner configuration
- OSI Layer 1/2/3 device distinctions
- Practical hub vs switch coexistence in one topology (PCs on switch side, 
  laptops on hub side, both terminating at the same router)
  
