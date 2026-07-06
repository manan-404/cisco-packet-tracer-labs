# Network Devices & CLI Basics

## Objective
Configure a multi-device network via router/switch CLI, understand basic device 
identification commands, and distinguish between core networking devices at 
different OSI layers.

## Topology
- Devices used: [fill in — routers/switches/PCs count from your actual topology]
- Topology diagram: see `screenshots/topology-diagram.png`

## Configuration Steps
1. Assigned IP addresses via CLI (not just GUI) on router/switch interfaces.
2. Set device hostname:
# Router(config)#hostname R1
3. Configured a Message of the Day (MOTD) banner:
Router(config)#banner motd #Authorized Access Only#


## Verification
- Hostname change reflected in CLI prompt — screenshot: `screenshots/hostname-config.png`
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
