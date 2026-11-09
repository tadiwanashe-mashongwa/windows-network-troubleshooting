# 4. TCP Ports and Process Investigation

## Objective

The objective of this exercise was to investigate TCP connectivity, identify listening ports on Windows and associate network connections with the processes responsible for them.

## 4.1 Testing TCP Connectivity

### Command

```powershell
Test-NetConnection google.com -Port 443
```

### Purpose

Tests whether a TCP connection can be established with a remote host on a specified port.

### Practical tests

The following tests were performed:

| Destination | Port | Result |
|---|---:|---|
| google.com | 443 | Successful |
| google.com | 22 | Failed |
| 142.250.130.113 | 443 | Successful |

### Findings

- TCP port 443 successfully established a connection.
- TCP port 22 failed to establish a connection to the tested Google destination.
- A direct IPv4 connection to port 443 also succeeded.

### Understanding the results

- **Port 443:** Commonly used for HTTPS traffic.
- **Port 22:** Commonly used for SSH connections.
- `TcpTestSucceeded: True` indicates that the TCP connection was successfully established.
- `TcpTestSucceeded: False` indicates that the connection could not be established.

A failed connection to one port does not necessarily mean the entire network is unavailable. The destination may not be listening on that port, or a firewall or network policy may be filtering the connection.

---

## 4.2 Identifying Listening TCP Ports

### Command

```powershell
Get-NetTCPConnection -State Listen |
Format-Table LocalAddress, LocalPort, State, OwningProcess -AutoSize
```

### Purpose

Displays TCP endpoints currently listening for incoming connections.

### Important fields

| Field | Meaning |
|---|---|
| LocalAddress | Local IP address associated with the endpoint |
| LocalPort | Local TCP port |
| State | Current TCP connection state |
| OwningProcess | Process ID (PID) associated with the endpoint |

### Practical findings

The investigation identified several listening ports:

| Port | Associated process |
|---:|---|
| 5432 | PostgreSQL |
| 49676 | MySQL |
| 2179 | Hyper-V VM management process |
| 445 | System |
| 55069 | TeamViewer |
| 12786 | Java / IntelliJ Maven server |

Some endpoints were listening on localhost (`127.0.0.1`), while others were listening on broader local interfaces.

### Understanding localhost

An application listening on `127.0.0.1` accepts connections through the local loopback interface.

This means the endpoint is intended for local communication rather than direct connections from other network devices.

---

## 4.3 Mapping Ports to Processes

### Command

```powershell
Get-Process -Id 1052,7832,3068,4,12952,7284,35588 |
Format-Table Id, ProcessName, Path
```

### Purpose

Maps process IDs to running Windows processes.

### Practical findings

The investigation identified processes including:

- PostgreSQL (`postgres.exe`)
- MySQL (`mysqld.exe`)
- TeamViewer (`TeamViewer.exe`)
- Hyper-V Virtual Machine Management (`vmms.exe`)
- Java (`java.exe`)
- Windows System process

### Understanding Process IDs

A Process ID (PID) is a numeric identifier assigned to a running process.

Windows uses PIDs to distinguish between processes.

The OwningProcess field from `Get-NetTCPConnection` can be used to identify which process owns a listening TCP endpoint.

---

## 4.4 Investigating a Java Process

### Command

```powershell
(Get-CimInstance Win32_Process -Filter "ProcessId = 35588").CommandLine
```

### Purpose

Retrieves the command-line information used to start a specific process.

### Practical findings

The Java process was associated with IntelliJ IDEA Community Edition and its embedded Maven server.

The process was listening on:

```text
127.0.0.1:12786
```

### What this demonstrated

- Java processes can own listening TCP ports.
- IntelliJ IDEA uses background processes for development-related operations.
- A Java process is not necessarily a running Spring Boot application.
- The process command line can provide additional information when investigating an unfamiliar process.

---

## 4.5 Troubleshooting Scenario

### Problem

A user reports:

"My application cannot connect to the database."

### Initial investigation

Check whether the database port accepts TCP connections:

```powershell
Test-NetConnection localhost -Port 5432
```

Inspect listening TCP endpoints:

```powershell
Get-NetTCPConnection -State Listen
```

Identify the process associated with a listening port:

```powershell
Get-Process -Id <PID>
```

### What to investigate

1. Is the database server running?
2. Is it listening on the expected TCP port?
3. Is the application connecting to the correct hostname and port?
4. Is a firewall or network policy blocking the connection?
5. Does the database process own the expected listening endpoint?

### Conclusion

TCP connectivity testing and process investigation help technicians distinguish between network connectivity problems, unavailable services and application-level connection issues.

Understanding listening ports and process ownership is an important skill when troubleshooting Windows applications and services.
