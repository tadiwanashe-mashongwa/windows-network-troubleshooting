# 5. Practical Network Troubleshooting Scenarios

## Objective

This document presents practical troubleshooting scenarios based on the network diagnostic exercises performed throughout this project.

The goal is to demonstrate a structured approach to identifying possible network problems using built-in Windows tools.

---

## Scenario 1: Connected to Wi-Fi but Cannot Access Websites

### Problem

A user reports that their computer is connected to Wi-Fi, but websites are not opening.

### Investigation

Inspect network configuration:

```powershell
ipconfig /all
```

Test the default gateway:

```cmd
ping 192.168.0.1
```

Test external IP connectivity:

```cmd
ping 8.8.8.8
```

Test domain resolution and connectivity:

```cmd
nslookup google.com
```

### Possible findings

- Gateway unreachable: investigate local connectivity.
- Gateway reachable but external IP unreachable: investigate router or upstream connectivity.
- External IP reachable but DNS lookup fails: investigate DNS configuration.

### Approach

Use the test results to narrow down the problem rather than immediately changing network settings.

---

## Scenario 2: DNS Resolution Failure

### Problem

A user can reach an external IP address but cannot access websites using domain names.

### Investigation

```cmd
nslookup google.com
```

Test an alternative resolver:

```cmd
nslookup google.com 8.8.8.8
```

### Interpretation

If the default resolver fails but the alternative resolver succeeds, investigate the configured DNS server.

If both fail, investigate DNS connectivity and the broader network path.

---

## Scenario 3: Slow Network or Suspected Packet Loss

### Problem

A user reports intermittent connectivity or slow network performance.

### Investigation

```cmd
ping 8.8.8.8
```

Trace the route:

```cmd
tracert 8.8.8.8
```

Investigate packet loss:

```cmd
pathping 8.8.8.8
```

### Interpretation

- Repeated packet loss can indicate a network problem.
- A non-responsive intermediate router does not automatically indicate packet loss.
- Compare results across multiple hops before identifying a suspected problem location.

---

## Scenario 4: Application Cannot Connect to a Service

### Problem

An application cannot establish a TCP connection to a remote service.

### Investigation

```powershell
Test-NetConnection google.com -Port 443
```

Inspect listening endpoints:

```powershell
Get-NetTCPConnection -State Listen
```

Identify the process:

```powershell
Get-Process -Id <PID>
```

### Interpretation

Investigate whether the destination service is available, whether the expected port is listening and whether a firewall or network policy may be blocking communication.

---

## Scenario 5: Identifying an Unknown Listening Port

### Problem

A technician discovers an unfamiliar listening TCP port and needs to identify the associated application.

### Investigation

```powershell
Get-NetTCPConnection -State Listen |
Format-Table LocalAddress, LocalPort, State, OwningProcess -AutoSize
```

Use the process ID to identify the application:

```powershell
Get-Process -Id <PID>
```

For further investigation:

```powershell
Get-CimInstance Win32_Process -Filter "ProcessId = <PID>" |
Select-Object ProcessId, Name, CommandLine
```

### Interpretation

Use the process name and command line to understand which application owns the listening endpoint.

Do not assume an unfamiliar port is malicious without further investigation.

---

## Troubleshooting Methodology

Throughout the exercises, the following approach was used:

1. Identify the reported problem.
2. Collect information using built-in Windows tools.
3. Test network configuration and connectivity.
4. Examine DNS resolution when necessary.
5. Investigate TCP connections and listening ports.
6. Interpret the evidence before recommending changes.

## Conclusion

Effective network troubleshooting requires a structured approach.

Windows PowerShell and built-in networking utilities provide useful diagnostic information that helps technicians investigate connectivity, DNS, routing and application communication problems.
