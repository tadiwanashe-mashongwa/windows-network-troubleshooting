# Windows Network Troubleshooting

## Project Overview

This project documents hands-on Windows network troubleshooting exercises performed using Windows PowerShell and built-in Windows networking utilities.

The goal is to develop practical IT Help Desk skills by learning how to inspect network configurations, diagnose connectivity problems, investigate DNS resolution, and troubleshoot TCP connections.

The exercises are based on practical testing and realistic technical support scenarios.

## Technologies and Tools

The following tools and utilities were used throughout the practical exercises:

### Operating System
- Windows 10/11

### Command-Line Environment
- Windows PowerShell
- Command Prompt (CMD)

### Network Configuration
- `ipconfig`
- `Get-NetAdapter`
- `Get-NetIPConfiguration`
- `route print`
- `arp`

### DNS Troubleshooting
- `nslookup`
- `Resolve-DnsName`
- `Get-DnsClientCache`

### Connectivity Testing
- `ping`
- `tracert`
- `pathping`

### TCP and Process Investigation
- `Test-NetConnection`
- `Get-NetTCPConnection`
- `Get-Process`
- `Get-CimInstance`

## 1. Network Configuration and Inspection

The first part of the project focused on understanding the computer's network configuration and identifying the network interfaces available on Windows.

### Commands Practised

| Command | Purpose |
|---|---|
| `ipconfig /all` | Displays detailed network configuration, including IP addresses, DNS servers, DHCP and default gateway. |
| `Get-NetAdapter` | Lists network adapters and their operational status. |
| `Get-NetIPConfiguration` | Displays IP configuration for network interfaces. |
| `route print -4` | Displays the IPv4 routing table. |
| `arp -a` | Displays the ARP cache, mapping local IPv4 addresses to MAC addresses. |

### Practical Findings

During the exercises, I identified:

- The active Wi-Fi network adapter.
- The computer's IPv4 address and subnet configuration.
- The default gateway used to reach external networks.
- Physical and virtual network adapters.
- The default route used for IPv4 traffic.
- The relationship between IP addresses and MAC addresses through ARP.

### Key Concepts Learned

- IPv4 addressing and subnet masks.
- Default gateways and routing.
- DHCP and DNS configuration.
- Physical versus virtual network adapters.
- ARP address resolution.
- How Windows determines where to send network traffic.
