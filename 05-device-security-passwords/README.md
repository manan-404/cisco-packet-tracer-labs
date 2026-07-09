# Device Security — Password Configuration

## Objective
Secure router access at multiple levels — console line, privileged EXEC mode 
(both unencrypted and encrypted), and remote VTY (Telnet) access — and verify 
that each layer actually enforces authentication.

## 1. Console Password
Router>enable
Router#configure terminal
Router(config)#line console 0
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#exit
Screenshot: `screenshots/01-console-password.png`

## 2. Enable Password (unencrypted)
Router>enable
Router#configure terminal
Router(config)#enable password cisco1
Router(config)#exit
Screenshot: `screenshots/02-enable-password.png`

## 3. Enable Secret (encrypted)
Router>enable
Router#configure terminal
Router(config)#enable secret cisco2
Router(config)#exit
Note: when both `enable password` and `enable secret` are set, **`enable secret` 
always takes priority** — confirmed by being prompted for it instead of the plain 
enable password after this step.
Screenshot: `screenshots/03-enable-secret.png`

## 4. VTY (Virtual Terminal / Telnet) Password
Router>enable
Router#configure terminal
Router(config)#line vty 0 4
Router(config-line)#password cisco3
Router(config-line)#login
Router(config-line)#exit
Screenshot: `screenshots/04-vty-password.png`

## Verification — Remote Telnet Access
1. Pinged router at `192.168.1.1` from PC — successful (TTL=255, 0% loss)
2. Telnetted into router (`telnet 192.168.1.1`) — prompted for **VTY password** first
3. Ran `enable` — prompted for **enable secret** (not the plain enable password), 
   confirming secret overrides password
4. Successfully reached privileged EXEC (`Router#`) and ran `show ?` to confirm access
Screenshot: `screenshots/05-telnet-verification.png`

## Concepts Covered
- Console line password vs privileged EXEC password vs VTY password — three 
  distinct access layers
- `enable password` vs `enable secret` — plaintext vs encrypted (MD5) storage, 
  and secret's priority over password
- Remote access security via Telnet + VTY authentication
- Practical proof that unauthenticated access is blocked at every layer



