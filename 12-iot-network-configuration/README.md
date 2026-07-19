# IoT Network Configuration

## Objective
Build an IoT device network using a wireless router and a registration/AAA 
server, register smart devices (fan, light) to the server, secure the 
wireless connection with WPA2 Enterprise + RADIUS, and control devices 
remotely through a web-based IoT monitor.

## Topology
- Server0 running IoT registration + AAA (RADIUS) services
- Switch connecting Server0, PC, and a Wireless Router (WRT300N)
- IoT end devices (Ceiling Fan, Light) connected wirelessly to the router
- Screenshots: `screenshots/01-iot-server-devices-topology.png`, 
  `screenshots/02-iot-devices-second-topology.png`

## Server Configuration
1. Enabled IoT service on Server0 (Services → IoT → On)
2. Assigned static IP to server: `192.168.0.2 / 255.255.255.0`
3. Enabled AAA service (Services → AAA), configured RADIUS client entry:
   - Client Name: Router, Client IP: `192.168.0.1`, Server Type: Radius, Key: `123`
4. Added device users under AAA User Setup (e.g. `fan` / `123`, `light` / `123`)
5. Created IoT Monitor registration account (username/password) via 
   Desktop → IoT Monitor → Sign up

## Wireless Router Configuration
1. Set SSID under Wireless → Basic Wireless Settings → Network Name: `Router`
2. Configured Wireless Security → Security Mode: **WPA2 Enterprise**
   - RADIUS Server: `192.168.0.2`
   - Shared Secret: `123`

## IoT Device Configuration (Fan, Light)
1. Interface → Wireless0 → SSID: `Router`
2. Authentication: WPA2, User ID/Password matching the AAA user entries 
   (e.g. `fan` / `123`)
3. IP obtained via DHCP from the wireless router
4. Repeated identical steps for the second IoT device (Light)

## Verification
- Opened PC's web browser → navigated to `http://192.168.0.2/home.html`
- Logged into the IoT Server's device dashboard
- Both registered devices (Ceiling Fan, Light) appeared online and controllable 
  (On/Off/Dim) directly from the browser interface

## Concepts Covered
- IoT service and AAA/RADIUS configuration on a Cisco Packet Tracer server
- WPA2 Enterprise wireless security (RADIUS-authenticated, not pre-shared key)
- IoT device registration and per-device credential-based authentication
- Remote device control via a web-based IoT monitor dashboard
- Practical example of AAA (Authentication, Authorization, Accounting) applied 
  to IoT/smart-home devices rather than traditional network user login

## Note
This lab's course topic is IoT device networking, not a dedicated wireless 
security lab — WPA2 Enterprise configuration appears here only as a supporting 
step required to authenticate IoT devices, not as standalone practice of 
wireless network design.
