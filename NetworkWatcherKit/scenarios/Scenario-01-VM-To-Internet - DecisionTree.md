# 🚨 Root Cause Decision Tree

```mermaid
flowchart TD

A["🌐 VM Cannot Reach Internet"] --> B{"🔍 Does DNS Resolution Work?"}

B -->|❌ No| C["DNS Issue<br/>• DNS Server Unreachable<br/>• Incorrect DNS Configuration<br/>• Private/Public DNS Resolution Problem"]

B -->|✅ Yes| D{"🛣️ What Does Azure Network Watcher Next Hop Show?"}

D -->|None| E["Route Issue<br/>• Blackhole Route<br/>• Missing UDR<br/>• Incorrect Route Table"]

D -->|VirtualNetworkGateway| F["Forced Tunneling<br/>• Traffic Sent On-Prem<br/>• No Internet Breakout"]

D -->|VirtualAppliance| G["Traffic Traversing Firewall / NVA"]

D -->|Internet| H["Routing Appears Healthy"]

G --> I{"🔥 Firewall Allows Traffic?"}

I -->|❌ No| J["Firewall Rule Issue<br/>• Network Rule Missing<br/>• Application Rule Missing<br/>• Policy Deny"]

I -->|✅ Yes| H

H --> K{"🛡️ Azure Network Watcher IP Flow Verify Result"}

K -->|Deny| L["NSG Blocking Traffic"]

K -->|Allow| M{"🌍 Explicit Outbound Path Available?"}

M -->|❌ No| N["Missing Outbound Connectivity<br/>• No NAT Gateway<br/>• No Public IP<br/>• No Load Balancer Outbound Rule<br/>• Private Subnet"]

M -->|✅ Yes| O{"💻 Guest OS Allows Traffic?"}

O -->|❌ No| P["OS-Level Issue<br/>• Windows Firewall<br/>• Linux iptables/nftables<br/>• NIC Configuration"]

O -->|✅ Yes| Q{"🚀 Application Layer Healthy?"}

Q -->|❌ No| R["Application Issue<br/>• Service Down<br/>• Port Not Listening<br/>• TLS/SSL Failure"]

Q -->|✅ Yes| S["✅ Connectivity Restored"]
```

## 🎯 Troubleshooting Logic

```text
1. DNS
      ↓
2. Routing
      ↓
3. Firewall / NVA
      ↓
4. NSG Evaluation
      ↓
5. Outbound Connectivity Method
      ↓
6. Guest Operating System
      ↓
7. Application Layer
```

## 🔎 Quick Root Cause Mapping

| Finding | Most Likely Cause |
|----------|------------------|
| DNS lookup fails | DNS configuration issue |
| Next Hop = None | Route table/UDR issue |
| Next Hop = VirtualNetworkGateway | Forced tunneling/on-prem routing |
| No Firewall logs found | Traffic never reached firewall |
| Firewall logs show Deny | Firewall policy issue |
| IP Flow Verify = Deny | NSG rule blocking traffic |
| NSG allows traffic but internet still unavailable | Missing outbound connectivity |
| No NAT Gateway/Public IP/Outbound Rule | Explicit outbound path missing |
| Network checks pass but access still fails | Guest OS firewall or application issue |

> [!TIP]
> **Golden Rule:** Always start with DNS, then Routing, then Security Controls. Network Watcher is most effective when used to eliminate one layer at a time rather than troubleshooting everything simultaneously.