# NAT & PAT (Network/Port Address Translation)

## Objective
Configure Dynamic NAT to translate multiple inside local addresses to a pool 
of global addresses, then configure PAT (NAT overload) to translate multiple 
inside hosts through a single outside interface IP, and verify both using 
translation tables.

## Topology
- Inside network: `192.168.10.0/24` (GigabitEthernet0/0/0, `ip nat inside`)
- Outside link: `50.1.1.0/8` via Serial0/1/0 (`ip nat outside`)
- Screenshots: `screenshots/01-topology.png`, `screenshots/02-nat-inside-outside-config.png`

## Dynamic NAT Configuration

ip nat pool global 50.1.1.1 50.1.1.254 netmask 255.255.255.0

ip nat inside source list 1 pool global

access-list 1 permit 192.168.10.0 0.0.0.255

ip route 0.0.0.0 0.0.0.0 50.1.1.2



**Interfaces:**

interface GigabitEthernet0/0/0

ip address 192.168.10.1 255.255.255.0

ip nat inside

interface Serial0/1/0

ip address 50.1.1.1 255.0.0.0

ip nat outside



**Verification — `show ip nat translations`:**
Pro  Inside global      Inside local        Outside local      Outside global

icmp 50.1.1.2:22        192.168.10.3:22     192.168.20.2:22    192.168.20.2:22

icmp 50.1.1.2:23        192.168.10.3:23     192.168.20.2:23    192.168.20.2:23

icmp 50.1.1.3:25        192.168.10.10:25    192.168.20.2:25    192.168.20.2:25

icmp 50.1.1.3:26        192.168.10.10:26    192.168.20.2:26    192.168.20.2:26

icmp 50.1.1.4:3         192.168.10.2:3      192.168.20.2:3     192.168.20.2:3

icmp 50.1.1.5:4         192.168.10.4:4      192.168.20.2:4     192.168.20.2:4


Each distinct inside host (`.10.2`, `.10.3`, `.10.4`, `.10.10`) is dynamically 
assigned a **different** outside global IP from the pool (`50.1.1.2` – 
`50.1.1.5`), confirming Dynamic NAT's 1-to-1 mapping behavior — one global 
address consumed per active inside host, not shared.

Screenshot: `screenshots/03-dynamic-nat-config.png`

## PAT (NAT Overload) Configuration
Same topology, single command change to enable port-based overload:
ip nat inside source list 1 interface Serial0/1/0 overload

**Verification — `show ip nat translations`:**
Pro  Inside global      Inside local     Outside local      Outside global

icmp 50.1.1.1:1024      192.168.10.5:1   192.168.20.2:1     192.168.20.2:1024

icmp 50.1.1.1:1025      192.168.10.4:1   192.168.20.2:1     192.168.20.2:1025

icmp 50.1.1.1:1         192.168.10.2:1   192.168.20.2:1     192.168.20.2:1

icmp 50.1.1.1:2         192.168.10.2:2   192.168.20.2:1     192.168.20.2:2



Unlike Dynamic NAT, **every** inside host now shares the single outside 
interface IP `50.1.1.1`, differentiated only by port number — this is the 
core PAT behavior (many-to-one via port multiplexing).

**Verification — `show ip nat statistics`:**

Total translations: 4 (0 static, 4 dynamic, 4 extended)

Outside Interfaces: Serial0/1/0

Inside Interfaces: GigabitEthernet0/0/0

Hits: 3   Misses: 4

## Dynamic NAT vs PAT — Key Difference
| Feature | Dynamic NAT | PAT (Overload) |
|---------|-------------|-----------------|
| Address mapping | 1 inside host → 1 unique outside IP (from pool) | Many inside hosts → 1 outside IP |
| Differentiator | IP address only | IP address + port number |
| Pool exhaustion | Possible if hosts > pool size | Not applicable — one IP shared via ports |
| Config directive | `ip nat inside source list 1 pool global` | `ip nat inside source list 1 interface <int> overload` |

## Concepts Covered
- NAT inside/outside interface designation
- Dynamic NAT with address pool configuration
- PAT (NAT overload) using a single outside interface IP
- `show ip nat translations` and `show ip nat statistics` verification
- Standard ACL as NAT's traffic-selection mechanism (`access-list 1 permit ...`)
- Practical distinction between Dynamic NAT's 1:1 mapping and PAT's port-based many:1 mapping
