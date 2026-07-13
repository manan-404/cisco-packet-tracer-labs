# Access Control Lists (ACLs)

## Objective
Configure standard ACLs to block specific hosts from reaching a target 
network, and verify each block using ping tests.

## Task 1: Block a host from reaching the target network
Router(config)#access-list 1 deny host 192.168.10.2
Router(config)#access-list 1 permit any
Router(config)#interface gigabitEthernet 0/0/0
Router(config-if)#ip access-group 1 out

**Verification:**
Router#show access-lists
Standard IP access list 1
10 deny host 192.168.10.2
20 permit any

A new PC was added to the denied network to confirm the host became 
unreachable.

Screenshots:
- `screenshots/01-exercise-intro.png`
- `screenshots/02-acl-config-task1.png`
- `screenshots/03-acl-verify-task1.png`
- `screenshots/04-denied-pc-unreachable.png`

## Task 2: Block a second, distinct host
Router(config)#access-list 2 deny host 192.168.40.2
Router(config)#access-list 2 permit any
Router(config)#interface gigabitEthernet 0/0/0
Router(config-if)#ip access-group 2 out

**Verification — ping to blocked host (192.168.40.2, PC3):**
C:>ping 192.168.40.2
Reply from 192.168.30.2: Destination host unreachable.
Reply from 192.168.30.2: Destination host unreachable.
Reply from 192.168.30.2: Destination host unreachable.
Reply from 192.168.30.2: Destination host unreachable.
Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)

**Control test — ping to a non-blocked host (192.168.50.2) for comparison:**
C:>ping 192.168.50.2
Reply from 192.168.50.2: bytes=32 time=17ms TTL=126
Reply from 192.168.50.2: bytes=32 time=14ms TTL=126
Reply from 192.168.50.2: bytes=32 time=12ms TTL=126
Reply from 192.168.50.2: bytes=32 time=11ms TTL=126
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

The contrast confirms the ACL is correctly blocking only the intended host 
(`192.168.40.2`), while unrelated hosts on other networks remain fully 
reachable.

Screenshots:
- `screenshots/05-acl-config-task2.png`
- `screenshots/06-ping-blocked-task2.png`

## Concepts Covered
- Standard ACL syntax (`access-list <num> deny/permit host <ip>`)
- Implicit `permit any` requirement (without it, all traffic is denied by the 
  implicit deny-all at the end of every ACL)
- Applying an ACL to an interface with direction (`ip access-group <num> out`)
- Verifying ACLs via `show access-lists`
- Practical proof of blocking via ping ("Destination host unreachable")
- Using a control test (a non-blocked host) to confirm the ACL isn't 
  over-blocking the network
