# Static Routing & VLSM

## Objective
Configure static routes across a 3-router network to achieve full connectivity 
between non-directly-connected subnets, and analyze a VLSM (Variable Length 
Subnet Masking) scenario to understand subnet address exhaustion.

## Topology
- 3 routers (R0, R1, R2), each with a directly connected LAN and serial links 
  connecting to neighboring routers
- Screenshot: `screenshots/01-topology.png`

## IP Configuration
- Screenshot: `screenshots/02-ip-config.png`

## Static Route Configuration & Verification

**Router 0** — `show ip route static`
S 192.168.30.0/24 [1/0] via 192.168.20.2
S 192.168.40.0/24 [1/0] via 192.168.20.2

**Router 1** — `show ip route static`
S 192.168.10.0/24 [1/0] via 192.168.20.1
S 192.168.40.0/24 [1/0] via 192.168.30.2

**Router 2** — `show ip route static`
S 192.168.10.0/24 [1/0] via 192.168.30.1
S 192.168.20.0/24 [1/0] via 192.168.30.1

Screenshot: `screenshots/03-static-routes-config.png`

Each router has static routes pointing to every subnet it isn't directly 
connected to, routed via the next-hop neighbor — full mesh reachability 
achieved with `[1/0]` administrative distance/metric (standard for static routes).

## VLSM Analysis — Subnet Exhaustion Exercise
**Problem:** Given network `192.168.10.0/25` (128 hosts total), accommodate 3 
separate subnets (S1, S2, S3) using VLSM.

**Subnet block reference:**
| CIDR | Hosts |
|------|-------|
| /25  | 128   |
| /26  | 64    |
| /27  | 32    |
| /28  | 16    |
| /29  | 8     |
| /30  | 4     |
| /31  | 2     |

**Allocation attempted:**
- S1: `192.168.10.0 – .63` (`/26`)
- S2: `192.168.10.64 – .95` (`/27`) — couldn't use `/28` since it wouldn't 
  leave room to also fit S3 given network/broadcast address overhead
- S3: `192.168.10.96 – .127` (`/27`)

**Conclusion:** This consumes the entire `/25` range (0–127) with only 3 
subnets. The original `192.168.10.0/25` allocation is **insufficient** for 
any additional subnets beyond these three, since address space is fully 
exhausted at `.127` — demonstrating a real VLSM constraint, not just a 
theoretical one.

## Concepts Covered
- Static routing configuration (`ip route`) across a multi-router network
- Route verification via `show ip route static`
- VLSM subnet block sizing and host-count tradeoffs
- Practical proof of subnet address exhaustion (network/broadcast overhead 
  reduces usable ranges more than raw host-count math suggests)
