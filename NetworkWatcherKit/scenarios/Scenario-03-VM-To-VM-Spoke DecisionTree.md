# 🚨 Hub-and-Spoke Connectivity Decision Tree

```mermaid
flowchart TD

A["🚨 VM in Spoke 1 Cannot Reach VM in Spoke 2"] --> B["🧪 Test Connectivity from Source VM<br/>Test-NetConnection / nc / ping"]

B --> C{"📍 Correct Destination IP & Port?"}

C -->|❌ No| D["⚙️ Fix Application Configuration<br/>Update Destination IP / Port"]

C -->|✅ Yes| E["🔗 Verify VNet Peerings"]

E --> F{"✅ Peerings Connected?"}

F -->|❌ No| G["🚫 Fix VNet Peering"]

F -->|✅ Yes| H["🛣️ Azure Network Watcher<br/>Next Hop"]

H --> I{"🎯 Next Hop = Virtual Appliance?"}

I -->|❌ No| J["🚧 Investigate UDRs<br/>Routing Issue"]

I -->|✅ Yes| K["📋 Review Effective Routes"]

K --> L{"✅ Route to Remote Spoke Exists?"}

L -->|❌ No| M["🚧 Missing or Incorrect Route"]

L -->|✅ Yes| N["🛡️ Azure Network Watcher<br/>IP Flow Verify"]

N --> O{"✅ Traffic Allowed?"}

O -->|❌ No| P["🚫 NSG Blocking Traffic"]

O -->|✅ Yes| Q["🌐 Azure Network Watcher<br/>Connection Troubleshoot"]

Q --> R{"✅ Reachable?"}

R -->|❌ No| S["🔥 Hub Firewall / NVA Investigation"]

R -->|✅ Yes| T["💻 Inspect Destination VM"]

T --> U{"✅ Service Listening?"}

U -->|❌ No| V["⚙️ Fix Application / Service"]

U -->|✅ Yes| W["🛡️ Check Guest OS Firewall"]

W --> X{"✅ Firewall Allows Traffic?"}

X -->|❌ No| Y["🚫 Windows Firewall / Linux Firewall"]

X -->|✅ Yes| Z["📦 Collect Network Trace"]

Z --> AA["🔬 Analyze Packets<br/>SYN / SYN-ACK / RST"]

AA --> BB["🎯 Identify Root Cause"]
```

---

## 🎮 Fast-Track Troubleshooting Path

```text
🚨 Connectivity Issue
        ↓
🧪 Test from Source VM
        ↓
🔗 Verify Peering
        ↓
🛣️ Next Hop
        ↓
📋 Effective Routes
        ↓
🛡️ IP Flow Verify
        ↓
🌐 Connection Troubleshoot
        ↓
💻 Guest OS
        ↓
📦 Packet Capture
```

---

## 🔍 Quick Root Cause Mapping

| Symptom | Most Likely Cause |
|----------|----------|
| Peering not Connected | VNet peering issue |
| Next Hop ≠ VirtualAppliance | Routing/UDR issue |
| No route to remote spoke | Missing Effective Route |
| IP Flow Verify = Deny | NSG issue |
| Connection Troubleshoot = Unreachable | Routing, Firewall, or Peering issue |
| Forward works, return fails | Asymmetric routing / return path issue |
| Reachable but application fails | Guest OS or application issue |
| Traffic arrives but no response | Service not listening |
| Traffic blocked on VM | Windows/Linux firewall |

---

> [!TIP]
>
> 🧠 **Golden Rule for Hub-and-Spoke Troubleshooting**
>
> Always verify the path in **both directions**:
>
> ```text
> Spoke 1 ➜ Hub ➜ Spoke 2 ✅
>
> Spoke 2 ➜ Hub ➜ Spoke 1 ✅
> ```
>
> Most Hub-and-Spoke issues are not caused by the forward path, but by a missing **return route**, disabled **Allow Forwarded Traffic**, or an incorrect **UDR association**.