# 🕵️ Azure Network Troubleshooting Methodology

> [!TIP]
> Every successful investigation should identify **where traffic starts, where it should go, how it should get there, and what is preventing it from reaching the destination.**

---

## 🎯 1. Reproduce the Issue

Before analyzing Azure resources, confirm the issue is actually occurring.

✅ Validate from the source system

✅ Capture the exact error message

✅ Confirm whether the issue is intermittent or consistent

✅ Identify if the issue affects a single system or multiple systems

> **Never troubleshoot assumptions. Troubleshoot observations.**

---

## 🏗️ 2. Understand the Architecture

Map the complete network path before opening troubleshooting tools.

### Questions to Ask

- Where is the source?
- Where is the destination?
- What connectivity method is expected?

### Components to Identify

- 🌐 Virtual Networks
- 🔗 VNet Peering
- 🔥 Azure Firewall
- ⚖️ Load Balancer
- 🚪 Application Gateway
- 📡 NAT Gateway
- 🔒 VPN Gateway
- 🚄 ExpressRoute
- 🖧 Network Virtual Appliances (NVAs)

---

## 🛣️ 3. Identify the Traffic Path

Understand every hop the traffic must traverse.

### Verify

- Route Tables (UDRs)
- Azure Firewall
- NVAs
- Load Balancers
- VPN Gateways
- ExpressRoute Gateways

### Ask

✅ Which path should traffic take?

✅ Which path is traffic actually taking?

✅ Is the expected next hop correct?

---

## 🌍 4. Validate DNS Resolution First

If the destination is an FQDN, always verify DNS before investigating networking.

### Common Checks

```bash
nslookup storageaccount.blob.core.windows.net

Resolve-DnsName storageaccount.blob.core.windows.net
```

### Verify

- ✅ Custom DNS Servers
- ✅ Azure DNS Private Resolver
- ✅ Azure Private DNS Zones
- ✅ Public DNS Records
- ✅ Expected IP Resolution

> [!WARNING]
> A large percentage of "network issues" are actually DNS resolution issues.

---

## 🚦 5. Separate Routing from Filtering

### Routing Questions

- Is there a valid route?
- What is the Next Hop?
- Does the destination network exist?

### Filtering Questions

- Is traffic being denied?
- Which NSG rule matched?
- Which Firewall rule matched?
- Is a packet being dropped?

### Azure Network Watcher Tools

| Area | Tool |
|--------|--------|
| Routing | Next Hop |
| Routing | Effective Routes |
| Security | IP Flow Verify |
| Connectivity | Connection Troubleshoot |
| Verification | Packet Capture |

---

## 💻 6. Understand the Operating Systems

Azure networking may be functioning correctly while the guest operating system is blocking traffic.

### Windows

- Windows Defender Firewall
- Windows Routing Table
- Service Listening State

### Linux

- iptables
- nftables
- firewalld
- Local Routing Table

### Verify

```bash
netstat -ano

ss -lntp

route print

ip route
```

---

## 🔄 7. Verify Bidirectional Communication

Most applications require successful communication in both directions.

### Validate

```text
Source → Destination ✅

Destination → Source ✅
```

### Investigate

- Asymmetric routing
- Missing return routes
- Firewall return traffic behavior

> [!IMPORTANT]
> Successful outbound traffic does not guarantee successful inbound return traffic.

---

## 🚀 8. Validate the Application Layer

Network connectivity does not always mean the application is healthy.

### Verify

- Port Accessibility
- Service Availability
- TLS/SSL Requirements
- Backend Health
- Certificate Validity

### Examples

```bash
curl https://app.contoso.com

Test-NetConnection server01 -Port 443
```

---

## 📊 9. Use Evidence, Not Assumptions

Every conclusion should be backed by evidence.

### Evidence Sources

- 🔍 Network Watcher Output
- 🛣️ Route Tables
- 🛡️ NSG Rules
- 🔥 Firewall Logs
- 📦 Packet Captures
- 📄 Diagnostic Logs

---

## 🧩 10. Isolate One Layer at a Time

Avoid troubleshooting everything simultaneously.

### Investigation Flow

```text
DNS
 ↓
Routing
 ↓
Security Filtering
 ↓
Transport (TCP / UDP)
 ↓
Application
```

This approach reduces the troubleshooting scope quickly.

---

## ⚖️ 11. Compare Expected vs Actual

Every incident should compare intended behavior against actual behavior.

| Expected | Actual |
|-----------|-----------|
| Route | Route |
| Next Hop | Next Hop |
| IP Address | IP Address |
| DNS Resolution | DNS Resolution |
| NSG Rule | Matched Rule |
| Firewall Policy | Applied Policy |

The root cause is usually hidden in the differences.

---

## 📝 12. Record Findings

A good engineer documents evidence.

### Capture

- Tool Used
- Command Executed
- Result Obtained
- Matched Rule
- Root Cause
- Resolution

### Example

```text
Tool:
IP Flow Verify

Result:
Denied

Matched Rule:
Deny-All-Inbound

Root Cause:
NSG rule priority issue

Resolution:
Added Allow-RDP rule with higher priority.
```

---

# ✅ The Four Questions Every Investigation Must Answer

| Question | Purpose |
|-----------|-----------|
| 📍 Where is the traffic originating? | Identify the source |
| 🎯 Where is the traffic supposed to go? | Identify the destination |
| 🛣️ What path should it take? | Validate routing |
| 🚧 What component is preventing access? | Identify the root cause |

---

> [!NOTE]
> **Azure Networking troubleshooting is not about memorizing tools. It is about understanding traffic flow, eliminating possibilities, and using evidence to identify the exact point of failure.**