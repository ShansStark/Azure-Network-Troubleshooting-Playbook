# 🚀 Azure Network Watcher Troubleshooting Kit

A practical collection of Azure networking troubleshooting scenarios built around real-world support investigations.

Instead of focusing only on Azure Network Watcher tools, each scenario follows the same troubleshooting methodology used by Azure networking engineers:

```text
Problem
        ↓
Reproduce Issue
        ↓
Understand Traffic Flow
        ↓
Azure Network Watcher
        ↓
Investigate Findings
        ↓
Root Cause
        ↓
Resolution
```

---

## 🎯 Repository Goals

This project aims to help engineers learn:

✅ Azure Network Watcher

✅ Network Troubleshooting Methodology

✅ Azure Firewall / NVA Troubleshooting

✅ VM Connectivity Troubleshooting

✅ Routing and NSG Analysis

✅ Private Endpoint Diagnostics

✅ DNS vs Connectivity Analysis

✅ Real-World Azure Support Investigation Techniques

---

## 🚀 Quick Navigation

### 📖 Getting Started

- [🧠 Investigation Methodology](scenarios/InvestigationMindset.md)
- [📚 Azure Network Watcher Overview](docs/NetworkWatcherOverview.md)
- [📤 Publishing to GitHub](docs/PublishToGitHub.md)

---

## 📚 Available Scenarios

| # | Scenario | Documentation | Decision Tree |
|---|----------|---------------|---------------|
| 01 | 🌐 VM to Internet Connectivity | [Open](scenarios/Scenario-01-VM-To-Internet.md) | [Decision Tree](scenarios/Scenario-01-VM-To-Internet-DecisionTree.md) |
| 02 | 🖥️ VM to VM (Same Virtual Network) | [Open](scenarios/Scenario-02-VM-To-VM-Same-VNet.md) | [Decision Tree](scenarios/Scenario-02-VM-To-VM-Same-VNet-DecisionTree.md) |
| 03 | 🔗 VM to VM (Peered VNets) | [Open](scenarios/Scenario-03-VM-To-VM-Spoke.md) | [Decision Tree](scenarios/Scenario-03-VM-To-VM-Spoke-DecisionTree.md) |
| 04 | 🔥 VM to VM via Azure Firewall / NVA | [Open](scenarios/Scenario-04-VM-To-VM-Via-Firewall.md) | [Decision Tree](scenarios/Scenario-04-VM-To-VM-Via-Firewall-DecisionTree.md) |
| 05 | 🔒 VM to FQDN / Private Endpoint | [Open](scenarios/Scenario-05-VM-To-FQDN-PrivateEndpoint.md) | [Decision Tree](scenarios/Scenario-05-VM-To-FQDN-PrivateEndpoint-DecisionTree.md) |

---

## 🧠 Investigation Methodology

Every scenario follows the same troubleshooting mindset.

📖 **Read First:**

➡️ [Investigation Mindset](scenarios/InvestigationMindset.md)

A successful investigation should answer four questions:

### 1️⃣ Where is the traffic originating?

Identify the source machine, source subnet, and source network.

### 2️⃣ Where is the traffic supposed to go?

Identify the destination endpoint, port, protocol, and service.

### 3️⃣ What path should the traffic take?

Understand:

- Routes
- NSGs
- VNet Peerings
- Azure Firewall / NVA
- DNS Resolution
- Private Endpoints

### 4️⃣ What component is preventing connectivity?

Gather evidence and eliminate possibilities methodically.

---

## 🔧 Azure Network Watcher Tools Covered

| Tool | Purpose | Microsoft Learn |
|--------|----------|----------|
| 🔍 Next Hop | Determine how Azure routes traffic | https://learn.microsoft.com/en-us/azure/network-watcher/diagnose-vm-network-routing-problem |
| 🛡️ IP Flow Verify | Identify NSG allow/deny decisions | https://learn.microsoft.com/en-us/azure/network-watcher/ip-flow-verify-overview |
| 🌐 Connection Troubleshoot | Validate end-to-end connectivity | https://learn.microsoft.com/en-us/azure/network-watcher/connection-troubleshoot-manage?tabs=portal |
| 🗺️ Topology | Visualize Azure networking resources | https://learn.microsoft.com/en-us/azure/network-watcher/view-network-topology |
| 📦 Packet Capture | Collect packet captures from Azure VMs | https://learn.microsoft.com/en-us/azure/network-watcher/packet-capture-overview |
| 📚 Network Watcher Overview | Overview of all Azure Network Watcher capabilities | https://learn.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview |

---

## 📖 Additional Azure Networking References

### 🔥 Azure Firewall

- Azure Firewall Overview
  - https://learn.microsoft.com/en-us/azure/firewall/overview

- Azure Firewall Monitoring & Logs
  - https://learn.microsoft.com/en-us/azure/firewall/monitor-firewall

- Azure Firewall Rule Processing
  - https://learn.microsoft.com/en-us/azure/firewall/rule-processing

---

### 🔗 Virtual Network Peering

- VNet Peering Overview
  - https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview

---

### 🛣️ User Defined Routes

- Azure Route Tables
  - https://learn.microsoft.com/en-us/azure/virtual-network/manage-route-table

- Azure Routing Overview
  - https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview

---

### 🛡️ Network Security Groups

- NSG Overview
  - https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview

- NSG Security Rules
  - https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview#security-rules

---

### 🔒 Private Endpoints

- Azure Private Endpoint Overview
  - https://learn.microsoft.com/en-us/azure/private-link/private-endpoint-overview

- DNS Configuration for Private Endpoints
  - https://learn.microsoft.com/en-us/azure/private-link/private-endpoint-dns

---

### 📛 Private DNS

- Azure Private DNS Zones
  - https://learn.microsoft.com/en-us/azure/dns/private-dns-privatednszone

---

## 🏛️ Architecture Patterns Covered

### Scenario 01

```text
VM
 ↓
Internet
```

### Scenario 02

```text
VM
 ↓
Same VNet
 ↓
VM
```

### Scenario 03

```text
VM
 ↓
VNet Peering
 ↓
VM
```

### Scenario 04

```text
VM
 ↓
Azure Firewall / NVA
 ↓
VM
```

### Scenario 05

```text
VM
 ↓
DNS Resolution
 ↓
Private Endpoint
 ↓
Azure PaaS Service
```

---

## 📂 Repository Structure

```text
azure-network-watcher-troubleshooting-kit
│
├── README.md
│
├── scenarios
│   ├── InvestigationMindset.md
│   │
│   ├── Scenario-01-VM-To-Internet.md
│   ├── Scenario-01-VM-To-Internet-DecisionTree.md
│   │
│   ├── Scenario-02-VM-To-VM-Same-VNet.md
│   ├── Scenario-02-VM-To-VM-Same-VNet-DecisionTree.md
│   │
│   ├── Scenario-03-VM-To-VM-Spoke.md
│   ├── Scenario-03-VM-To-VM-Spoke-DecisionTree.md
│   │
│   ├── Scenario-04-VM-To-VM-Via-Firewall.md
│   ├── Scenario-04-VM-To-VM-Via-Firewall-DecisionTree.md
│   │
│   ├── Scenario-05-VM-To-FQDN-PrivateEndpoint.md
│   └── Scenario-05-VM-To-FQDN-PrivateEndpoint-DecisionTree.md
│
├── docs
│   ├── NetworkWatcherOverviewion Learners
- Anyone learning Azure networking troubleshooting

---

## 📌 Key Takeaway

The goal of this repository is not simply to learn Azure Network Watcher tools.

The goal is to learn how to troubleshoot Azure networking problems systematically:

```text
Understand the Architecture
          ↓
Understand the Traffic Flow
          ↓
Validate the Network Path
          ↓
Analyze the Results
          ↓
Identify the Root Cause
          ↓
Implement the Fix
```

---

## 📝 Disclaimer

This repository is intended for learning, lab environments, interview preparation, and troubleshooting practice.

Examples and outputs are illustrative and may vary based on service versions, Azure regions, and product updates.

Always validate production changes using current Microsoft documentation.

This is a personal learning project and is not affiliated with Microsoft.

---

## ⭐ Future Enhancements

Potential future scenarios:

- Azure Load Balancer
- Application Gateway
- NAT Gateway
- VPN Gateway
- ExpressRoute
- Azure Front Door
- Azure DNS Private Resolver
- Hybrid Connectivity Scenarios

---

## 📄 License

MIT License

See the LICENSE file for details.