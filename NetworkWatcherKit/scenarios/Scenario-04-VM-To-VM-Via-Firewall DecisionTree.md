# 🚨 Firewall / NVA Connectivity Decision Tree

```mermaid
flowchart TD

A["🚨 VM1 Cannot Reach VM2"] --> B["🧪 Test from VM1<br/>Test-NetConnection / nc / ping"]

B --> C{"📍 Correct Destination IP & Port?"}

C -->|❌ No| D["⚙️ Fix Application Configuration"]

C -->|✅ Yes| E["🛣️ Azure Network Watcher<br/>Next Hop"]

E --> F{"🎯 Next Hop = Firewall / NVA?"}

F -->|❌ No| G["🚧 UDR / Route Table Issue"]

F -->|✅ Yes| H["🛡️ Azure Network Watcher<br/>IP Flow Verify"]

H --> I{"✅ NSG Allows Traffic?"}

I -->|❌ No| J["🚫 NSG Blocking Traffic"]

I -->|✅ Yes| K["🌐 Azure Network Watcher<br/>Connection Troubleshoot"]

K --> L{"✅ Reachable?"}

L -->|✅ Yes| M["💻 Check Guest OS / Application"]

L -->|❌ No| N["🔥 Review Firewall / NVA Logs"]

N --> O{"🔥 Traffic Seen?"}

O -->|❌ No| P["🚧 Traffic Never Reached Firewall<br/>Routing / Peering Issue"]

O -->|✅ Yes| Q{"🔥 Firewall Rule Match?"}

Q -->|❌ Deny| R["🚫 Firewall / NVA Policy Issue"]

Q -->|✅ Allow| S["🔄 Validate Return Path"]

S --> T{"✅ Return Path via Firewall?"}

T -->|❌ No| U["⚠️ Asymmetric Routing"]

T -->|✅ Yes| V["💻 Investigate OS / Application"]

V --> W["📦 Collect Packet Capture"]

W --> X["🔬 Analyze TCP Handshake"]
```

---

## 🎯 Recommended Troubleshooting Flow

```text
Source VM Validation
         ↓
Azure Network Watcher - Next Hop
         ↓
Azure Network Watcher - IP Flow Verify
         ↓
Azure Network Watcher - Connection Troubleshoot
         ↓
Firewall / NVA Logs
         ↓
Return Path Validation
         ↓
Guest OS Validation
         ↓
Packet Capture
```

---

## 🔍 Quick Root Cause Mapping

| Finding | Likely Root Cause |
|----------|------------------|
| Next Hop ≠ VirtualAppliance | UDR issue |
| Next Hop = None | Missing route |
| IP Flow Verify = Deny | NSG blocking traffic |
| Connection Troubleshoot = Unreachable | Routing or Firewall issue |
| No Firewall Logs | Traffic never reached Firewall/NVA |
| Firewall Log = Deny | Firewall policy issue |
| Firewall Log = Allow but traffic still fails | Return path issue |
| One-way communication | Asymmetric routing |
| Reachable but application fails | Guest OS or Application issue |
| SYN reaches destination but no reply | OS Firewall / Service issue |

---

> [!TIP]
>
> 🧠 **Golden Rule**
>
> For any hub-spoke deployment using Azure Firewall or an NVA:
>
> ```text
> VM1
>   ↓
> Firewall / NVA ✅
>   ↓
> VM2 ✅
>
> VM2
>   ↓
> Firewall / NVA ✅
>   ↓
> VM1 ✅
> ```
>
> Both the forward path and return path must traverse the same inspection device. Most "it works one way" issues are caused by missing UDRs or asymmetric routing.