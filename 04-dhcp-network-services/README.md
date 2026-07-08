# DHCP & Network Services

## Objective
Configure a DHCP server to automatically assign IP addresses to clients, and 
explore key application-layer network services (FTP, TFTP, HTTP), along with 
their underlying protocol theory.

## Topology & IP Configuration
- Assigned IP addresses to both router interfaces
- Screenshot: `screenshots/01-router-ip-config.png`

## DHCP Configuration
1. Configured DHCP server — Pool name: `serverPool`, Network: `192.168.3.0`, 
   Default Gateway: `192.168.3.10` — `screenshots/02-dhcp-server-config.png`
2. Set DHCP pool settings (address range, lease) — `screenshots/03-dhcp-pool-settings.png`
3. Verified client received IP automatically via DHCP — `screenshots/04-dhcp-client-assigned.png`
4. Verified DHCP binding/status — `screenshots/05-dhcp-verification.png`

## Additional Services Explored
- **FTP:** configured and tested — `screenshots/06-ftp-service.png`
- **Hostname configuration** — `screenshots/07-hostname.png`
- **TFTP:** configured and tested — `screenshots/08-tftp-service.png`
- **HTTP service enabled** on server — `screenshots/09-http-service.png`

## Protocol Theory
| Protocol | Purpose |
|----------|---------|
| DHCP | Automatically assigns IP addresses, subnet masks, gateways, and other config to devices, removing the need for manual setup |
| WWW  | System of interlinked hypertext documents accessed via browsers over HTTP |
| DNS  | Translates human-readable domain names (e.g. www.google.com) into IP addresses |
| FTP  | Standard protocol for transferring files between computers over a network |
| TFTP | Lightweight, no-authentication version of FTP, used for small files like config/firmware transfers on local networks |
| SMTP | Protocol for sending/forwarding email between servers (outgoing mail standard) |
| Email | Digital message exchange, typically using SMTP (send) and POP3/IMAP (receive) |

## Client-Server Model
A client-server network has multiple client computers requesting services or 
resources from a centralized, more powerful server.
- **Client:** requests data, files, or applications
- **Server:** centralized system providing file sharing, databases, email, web hosting, etc.

## Concepts Covered
- DHCP server configuration and automatic IP assignment
- FTP, TFTP, HTTP service configuration
- Core application-layer protocol theory (DNS, SMTP, Email)
- Client-server networking model
