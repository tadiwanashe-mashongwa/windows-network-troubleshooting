# 3. Network Connectivity Testing

## Objective

The objective of this exercise was to test network connectivity, examine packet delivery and identify possible communication problems between the local computer and remote destinations.

## 3.1 Testing Connectivity with Ping

### Commands

```cmd
ping 192.168.0.1
ping 8.8.8.8
ping google.com
```

### Purpose

The `ping` command uses ICMP Echo Requests and Echo Replies to test whether a destination responds over the network.

### Practical tests

Three connectivity tests were performed:

1. Local default gateway: `192.168.0.1`
2. External IPv4 address: `8.8.8.8`
3. Domain name: `google.com`

### Practical findings

All three tests received successful replies with zero packet loss.

The domain-name test also demonstrated that the destination could be resolved and reached using IPv6.

---

## 3.2 Tracing Network Routes with Tracert

### Command

```cmd
tracert 8.8.8.8
```

### Purpose

Displays the network hops taken by packets as they travel towards a destination.

### Practical findings

The trace showed:

- The first hop was the local default gateway (`192.168.0.1`).
- The next hop belonged to the ISP.
- Subsequent hops included intermediate network providers and Google infrastructure.
- The destination was successfully reached.

Some intermediate hops displayed asterisks (`* * *`).

### Interpretation

An asterisk indicates that a router did not return the expected traceroute response within the timeout.

This does not necessarily indicate a connectivity failure. Some routers filter or deprioritize diagnostic responses while still forwarding normal traffic.

---

## 3.3 Investigating Packet Loss with Pathping

### Command

```cmd
pathping 8.8.8.8
```

### Purpose

Combines route tracing with repeated packet tests to help investigate packet loss along a network path.

### Practical findings

The investigation identified:

- The local computer as the starting point.
- The default gateway as the first network hop.
- The ISP as the next hop.
- An intermediate hop that did not respond to diagnostic probes.

The responding hops showed no packet loss in the observed results.

### Interpretation

A router failing to respond to diagnostic probes does not automatically mean it is dropping forwarded traffic.

Packet loss should be interpreted across the complete route rather than assuming that a single non-responsive hop is the source of a problem.

---

## 3.4 Troubleshooting Scenario

### Problem

A user reports:

"I can connect to Wi-Fi, but I cannot access the internet."

### Initial investigation

Test the local gateway:

```cmd
ping 192.168.0.1
```

Test external IP connectivity:

```cmd
ping 8.8.8.8
```

Test domain connectivity:

```cmd
ping google.com
```

If necessary, investigate the route:

```cmd
tracert 8.8.8.8
```

### Interpretation

| Result | Possible investigation |
|---|---|
| Gateway ping fails | Local network connection, adapter or gateway |
| Gateway succeeds, external IP fails | Router, ISP or upstream connectivity |
| External IP succeeds, domain test fails | DNS resolution |
| Destination responds but an intermediate hop times out | Possible ICMP filtering or rate limiting |

These tests provide evidence to help narrow down the source of a connectivity problem.

---

## Conclusion

Ping, tracert and pathping provide different perspectives on network connectivity.

Using them together helps a technician investigate local connectivity, external reachability, routing paths and possible packet loss.
