# 2. DNS Troubleshooting

## Objective

The objective of this exercise was to understand how DNS resolves domain names into IP addresses and how to investigate DNS-related problems using Windows command-line tools.

## 2.1 Testing DNS Resolution with nslookup

### Command

```cmd
nslookup google.com
```

### Purpose

Queries a DNS server to resolve a domain name into IP addresses.

### Additional test

```cmd
nslookup google.com 8.8.8.8
```

This performs the lookup using Google's public DNS resolver instead of the computer's default DNS server.

### Practical findings

Both the default DNS resolver and Google's public DNS resolver successfully resolved the domain.

The default resolver displayed `Server: UnKnown`. This did not mean DNS resolution had failed because the domain lookup still returned results.

---

## 2.2 Using Resolve-DnsName

### Command

```powershell
Resolve-DnsName google.com
```

### Purpose

Uses PowerShell to perform DNS queries and display DNS records.

### Record types observed

| Record | Purpose |
|---|---|
| A | Resolves a hostname to an IPv4 address |
| AAAA | Resolves a hostname to an IPv6 address |
| PTR | Maps an IP address back to a hostname |

---

## 2.3 Inspecting the DNS Cache

### Commands

```cmd
ipconfig /displaydns
```

Displays DNS records currently cached by the Windows DNS Client.

```powershell
Get-DnsClientCache
```

Displays DNS cache entries using PowerShell.

### Filtering cached records

```powershell
Get-DnsClientCache | Where-Object Entry -Like "*google.com*"
```

This filters the DNS cache to display entries associated with google.com.

### Practical findings

The cache contained multiple A and AAAA records for google.com.

The exercises also demonstrated that DNS records have a Time To Live (TTL), which indicates how long a record can remain cached.

---

## 2.4 Understanding DNS TTL

TTL specifies how long a DNS record may remain in a cache before it should be refreshed.

For example, if a cached record has a TTL of 120 seconds, approximately two minutes remain before its cache lifetime expires.

A cached DNS record can help avoid repeatedly querying DNS servers for the same information.

---

## 2.5 Troubleshooting Scenario

### Problem

A user reports:

"I can connect to Wi-Fi, but websites are not opening."

### Initial DNS investigation

```cmd
nslookup google.com
```

If the lookup fails, test another resolver:

```cmd
nslookup google.com 8.8.8.8
```

### Interpretation

- If both queries fail, investigate broader DNS connectivity and network issues.
- If the default resolver fails but the public resolver succeeds, investigate the configured DNS server or its availability.
- If DNS resolution succeeds, continue investigating connectivity and application-level issues.

### Conclusion

DNS troubleshooting helps determine whether a connectivity problem is related to name resolution rather than assuming the internet connection itself is unavailable.
