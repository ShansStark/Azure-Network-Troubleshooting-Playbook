# 🌐 Azure Network Watcher Overview

> [!NOTE]
>
> Azure Network Watcher is one of the most powerful troubleshooting toolsets available in Azure networking.
>
> This guide explains **what each tool does**, **when to use it**, and most importantly **when not to use it**.

---

# 🚀 What is Azure Network Watcher?

Azure Network Watcher is a regional Azure service that helps you:

✅ Monitor network resources

✅ Troubleshoot connectivity issues

✅ Analyze routing decisions

✅ Validate NSG behavior

✅ Capture and inspect packets

✅ Visualize Azure network topologies

✅ Investigate performance and connectivity problems

Think of Network Watcher as your Azure networking toolbox.

```text
Connectivity Issue
        ↓
Network Watcher
        ↓
Evidence
        ↓
Root Cause
```

---

# 🧰 Tool Selection Cheat Sheet

| 🤔 Question | 🛠️ Tool | 📚 Scenarios |
|------------|----------|----------|
| Can VM A reach destination B? | 🔍 Connection Troubleshoot | 01, 02, 03, 04, 05 |
| Where is Azure sending the traffic? | 🛣️ Next Hop | 01, 03, 04 |
| Is an NSG blocking traffic? | 🛡️ IP Flow Verify | 01, 02, 04, 05 |
| Which NSG rules apply to this NIC? | 🔐 Effective Security Rules | 01, 05 |
| Which routes apply to this NIC? | 🛣️ Effective Routes | 01, 03, 04 |
| How are resources connected? | 🗺️ Topology | 01, 03, 05 |
| Did packets actually arrive? | 📦 Packet Capture | 02, 05 |
| Is connectivity degrading over time? | 📈 Connection Monitor | Future Labs |
| Who is talking to my resources? | 📊 Flow Logs / Traffic Analytics | Future Labs |
| Is my VPN healthy? | 🔒 VPN Troubleshooting | Future Labs |

---

# 🎯 Quick Tool Mapping

```text
Routing Problem?
      ↓
🛣️ Next Hop

NSG Problem?
      ↓
🛡️ IP Flow Verify

Connectivity Problem?
      ↓
🌐 Connection Troubleshoot

Packet-Level Analysis?
      ↓
📦 Packet Capture

Architecture Validation?
      ↓
🗺️ Topology
```

---

# 🛑 What Network Watcher Does NOT Tell You

Understanding the limits of the tools is just as important as understanding the tools themselves.

### 💻 Guest Operating System

Network Watcher cannot identify:

- Windows Defender Firewall
- Linux iptables
- Linux nftables
- firewalld
- ufw
- Whether a service is actually listening

Example:

```text
IP Flow Verify = Allow ✅

Application Still Fails ❌

Look Inside The VM 🔍
```

---

### 🔥 Azure Firewall & NVA Decisions

IP Flow Verify only evaluates NSGs.

It cannot determine:

- Azure Firewall policy matches
- Firewall rule collections
- Palo Alto rules
- Fortinet rules
- Check Point rules
- NVA decisions

For those investigations:

✅ Review firewall logs

✅ Review policy configuration

✅ Verify packet counters

---

### 🌐 DNS Resolution

Network Watcher has no DNS diagnostic tool.

Use:

```powershell
Resolve-DnsName

nslookup
```

or

```bash
dig

nslookup
```

before opening Azure Portal.

---

### 🚀 Application Layer Problems

Network Watcher cannot diagnose:

- Authentication failures
- Database login issues
- TLS/SSL errors
- Certificate issues
- Application configuration problems

---

# 🧠 Investigation Mindset

A good troubleshooting session isn't about running every tool.

It's about eliminating possibilities.

### 1️⃣ Reproduce The Issue

Validate the issue from the source system.

✅ Verify the symptom

✅ Capture the exact error

✅ Test from the source

---

### 2️⃣ Understand The Architecture

Map the traffic flow:

```text
Source
   ↓
Route
   ↓
Firewall
   ↓
Destination
```

Understand:

- VNets
- Subnets
- Peerings
- Firewalls
- Private Endpoints
- Gateways

---

### 3️⃣ Separate Routing From Filtering

Ask:

```text
Can Azure route it?
```

If Yes:

```text
Can Azure allow it?
```

If Yes:

```text
Can the VM or application accept it?
```

This simple approach eliminates most false assumptions.

---

### 4️⃣ Validate Both Directions

Most networking issues occur because:

```text
Request ✅

Response ❌
```

Always check:

```text
Source ➜ Destination

Destination ➜ Source
```

especially in Hub-Spoke architectures.

---

### 5️⃣ Use Evidence, Not Assumptions

Collect:

✅ IP Addresses

✅ Matched Rules

✅ Route Information

✅ Connection Results

✅ Firewall Logs

✅ Packet Captures

The goal is to prove a cause, not guess one.

---

# 🔐 Permissions & Requirements

Before using Network Watcher, verify:

✅ Network Watcher exists in the region

✅ VM Agent Extension is installed (where required)

✅ Appropriate RBAC permissions exist

✅ Storage Account exists for packet captures

Common role:

```text
Network Contributor
```

---

# ⚠️ Things To Verify Before Publishing

Azure networking changes continuously.

Review the latest Microsoft documentation before publishing content.

### 📊 Flow Logs

Microsoft is transitioning from:

```text
NSG Flow Logs
```

toward

```text
Virtual Network Flow Logs
```

Always verify current guidance.

---

### 🌐 Default Outbound Access

New Azure virtual networks increasingly use:

```text
Private Subnets
```

which changes how internet connectivity works.

Relevant to:

✅ Scenario 01

---

### 🛠️ Portal Experience

Azure periodically changes:

- Tool names
- Navigation paths
- UI layout
- Workflow locations

Screenshots may become outdated.

---

### ⚡ PowerShell & Azure CLI

Always validate commands with:

```powershell
Get-Help <cmdlet> -Full
```

or

```bash
az <command> --help
```

using the installed version of your tooling.

---

# 📚 Learning Resources

### 🌐 Azure Network Watcher

https://learn.microsoft.com/azure/network-watcher/

### 🔥 Azure Firewall

https://learn.microsoft.com/azure/firewall/

### 🔍 Next Hop

https://learn.microsoft.com/azure/network-watcher/diagnose-vm-network-routing-problem

### 🛡️ IP Flow Verify

https://learn.microsoft.com/azure/network-watcher/ip-flow-verify-overview

### 🌐 Connection Troubleshoot

https://learn.microsoft.com/azure/network-watcher/connection-troubleshoot-manage

### 🗺️ Network Topology

https://learn.microsoft.com/azure/network-watcher/view-network-topology

### 🔒 Private Endpoint DNS

https://learn.microsoft.com/azure/private-link/private-endpoint-dns

---

# 🎓 Final Thoughts

Network Watcher is most effective when used as part of a troubleshooting process:

```text
Understand Architecture
          ↓
Validate DNS
          ↓
Validate Routing
          ↓
Validate Security Controls
          ↓
Validate Connectivity
          ↓
Validate Guest OS
          ↓
Validate Application
```

> 💡 **Golden Rule**
>
> Don't start with the tool.
>
> Start with understanding how the traffic is supposed to flow, then use Network Watcher to prove where that flow breaks.