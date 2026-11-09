# 1. Network Configuration and Inspection

## Objective

The objective of this exercise was to inspect the Windows network configuration, identify active network interfaces, understand IP addressing and examine how Windows determines where network traffic should be sent.

## 1.1 Inspecting Network Configuration

### Command

```powershell
ipconfig /all
```

### Purpose

Displays detailed network configuration for all available network adapters.

### Information investigated

- IPv4 and IPv6 addresses
- Subnet mask
- Default gateway
- DHCP status
- DNS servers
- Physical (MAC) address

### Practical understanding

The default gateway is the device Windows uses to reach destinations outside its local network.

The subnet mask determines which IP addresses belong to the local network.

DNS servers translate domain names into IP addresses.

DHCP allows network configuration to be assigned automatically.

---

## 1.2 Identifying Network Adapters

### Command

```powershell
Get-NetAdapter
```

### Purpose

Lists network adapters and displays their names, interface descriptions, status and link information.

### Additional inspection

```powershell
Get-NetIPConfiguration
```

This displays IP configuration for network interfaces, including IPv4 addresses and gateway information.

### Practical findings

The computer contained several network interfaces, including:

- Physical Wi-Fi adapter
- Physical Ethernet adapter
- VirtualBox host-only adapters
- VMware virtual adapters
- Hyper-V virtual adapter
- Bluetooth networking adapter

### Troubleshooting relevance

A computer may have multiple network adapters. A technician should identify which adapter is active and which interface is being used before investigating connectivity problems.

---

## 1.3 Inspecting the Routing Table

### Command

```cmd
route print -4
```

### Purpose

Displays the Windows IPv4 routing table.

### Important routing concepts

| Destination | Meaning |
|---|---|
| Network destination | Destination network being matched |
| Netmask | Determines the size of the destination network |
| Gateway | Next-hop address |
| Interface | Local interface used to send traffic |
| Metric | Routing preference value |

### Default route

The default IPv4 route is represented by:

```text
0.0.0.0    0.0.0.0
```

It is used when no more specific route matches the destination.

Windows generally selects the most specific matching route, with route metrics helping determine preference between competing routes.

---

## 1.4 Inspecting the ARP Cache

### Command

```cmd
arp -a
```

### Purpose

Displays the Address Resolution Protocol (ARP) cache.

ARP helps Windows resolve an IPv4 address into a MAC address on the local network.

### Practical exercise

The ARP cache was cleared and the default gateway was pinged. The gateway's ARP entry subsequently appeared again.

### What this demonstrated

- ARP resolves local IPv4 addresses to MAC addresses.
- The computer needs the MAC address of its next hop to deliver an Ethernet frame.
- When communicating with an external network, the computer normally resolves the default gateway's MAC address rather than the remote server's MAC address.

### Important distinction

ARP does not track application ports. It handles local IP-to-MAC address resolution, while transport-layer protocols such as TCP use port numbers to identify application endpoints.

---

## 1.5 Troubleshooting Scenario

### Problem

A user reports that their computer cannot access websites.

### Initial investigation

A technician can begin with:

```powershell
ipconfig /all
Get-NetAdapter
Get-NetIPConfiguration
route print -4
```

### What to investigate

1. Is the network adapter enabled and connected?
2. Does the computer have a valid IP address?
3. Is the subnet configuration correct?
4. Is a default gateway configured?
5. Are DNS servers configured?
6. Is there an appropriate default route?

### Conclusion

Network configuration inspection provides a starting point for diagnosing connectivity problems before moving on to DNS and connectivity testing.
