# Spanning Tree Protocol (STP)

## Objective
Understand how STP prevents Layer 2 loops in a switched network, observe its 
effect with and without it enabled, manipulate root bridge election via 
priority and dedicated commands, and compare STP variants (RSTP, PVRST, 
PVRST+, MST).

## STP Enabled vs Disabled
- **With STP enabled:** redundant links between switches are automatically 
  placed in a blocking state to prevent loops, while still providing failover 
  if the active path goes down — `screenshots/01-stp-enabled-topology.png`
- **Without STP:** all redundant links remain active simultaneously, creating 
  the conditions for a broadcast storm / loop — `screenshots/02-stp-disabled-topology.png`

## Root Bridge Election
### Method 1 — Manual priority
Switch(config)#spanning-tree vlan 1 priority 4096
Lowers the bridge priority to force this switch to become the new root bridge 
(lower priority wins root election). Screenshot: `screenshots/03-priority-4096-new-root.png`

### Method 2 — Root primary/secondary shortcut
Switch(config)#spanning-tree vlan 1 root primary
Automatically sets a priority low enough to win root election without 
calculating the exact value manually. Screenshot: `screenshots/04-root-primary-command.png`
Switch(config)#spanning-tree vlan 1 root secondary
Configures a backup root bridge in case the primary root fails. Screenshot: `screenshots/05-secondary-root-setup.png`

## Verified Topology Result (4 switches)
| Switch | Bridge Priority | Role | Root Port | Cost to Root |
|--------|------------------|------|-----------|----------------|
| Switch0 | 24577 | **Root Bridge** | — | 0 |
| Switch1 | 32769 | Non-root | Fa0/3 | 19 |
| Switch2 | 32769 | Non-root | Fa0/2 | 19 |
| Switch3 | 32769 | Non-root | Fa0/2 | 38 (Fa0/1 in **Alternate Blocking** state) |

Confirmed via `show spanning-tree summary`, `show spanning-tree detail`, and 
`show spanning-tree active` on each switch — Switch3's Fa0/1 port sits in 
**blocking state** to prevent a loop, while still being available as a backup 
path if the active link fails.

## Theory: STP Variants
**STP:** Original loop-prevention protocol. Convergence is slow — 30–50 seconds 
after a topology change.

**RSTP (Rapid STP):** Faster version of STP, converges in a few seconds using 
improved port roles/states.

**PVRST (Per-VLAN Rapid STP):** Runs a separate RSTP instance per VLAN — 
flexible, but resource-heavy with many VLANs.

**PVRST+:** Cisco's refined PVRST with 802.1Q compatibility, making it 
practical in mixed-vendor environments.

**MST (Multiple STP):** Groups multiple VLANs into a single spanning tree 
instance, saving CPU/memory — more scalable for large networks.

Screenshots: `screenshots/06-q1-answer.png`, `screenshots/07-q2-answer.png`

## Enabling PVRST Mode
Switch(config)#spanning-tree mode rapid-pvst
Run on each switch individually. Screenshot: `screenshots/08-pvrst-mode-command.png`

## Concepts Covered
- STP loop prevention (blocking vs forwarding vs root port roles)
- Root bridge election via priority manipulation
- `root primary` / `root secondary` shortcut commands
- Verification via `show spanning-tree summary/detail/active`
- STP protocol family comparison (STP, RSTP, PVRST, PVRST+, MST)
