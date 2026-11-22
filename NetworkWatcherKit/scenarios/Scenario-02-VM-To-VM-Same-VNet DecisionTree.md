# 🚨 VM-to-VM Connectivity Decision Tree

```mermaid
flowchart TD

A["❌ VM1 Cannot Reach VM2"] --> B["🧪 Validate from VM1<br/>Test-NetConnection / nc / ping"]

B --> C{"Port Reachable?"}

C -->|✅ Yes| D["✅ Network Connectivity Healthy<br/>Investigate Application"]

C -->|❌ No| E["🛡️ Azure Network Watcher<br/>IP Flow Verify"]

E --> F{"NSG Result?"}

F -->|Deny| G["❌ NSG Blocking Traffic<br/>Review Matched Rule"]

F -->|Allow| H["🔎 Azure Network Watcher<br/>Connection Troubleshoot"]

H --> I{"Connection Status?"}

I -->|Reachable| J["✅ Azure Network Path Healthy"]

I -->|Unreachable| K["💻 Investigate Destination VM"]

K --> L["Check Windows Firewall / Linux Firewall"]

L --> M["Verify Service is Listening"]

M --> N{"Issue Found?"}

N -->|Yes| O["✅ Fix Guest OS or Service Configuration"]

N -->|No| P["📦 Collect Network Trace<br/>NetMon / netsh / tcpdump"]

J --> Q["🚀 Validate Application Configuration<br/>Authentication / TLS / App Settings"]

P --> R["🔬 Analyze Traffic Flow"]
```

---

## 🎯 Azure Networking Troubleshooting Flow

```text
Source VM Validation
         ↓
Azure Network Watcher - IP Flow Verify
         ↓
Azure Network Watcher - Connection Troubleshoot
         ↓
Guest OS Firewall
         ↓
Application Listening
         ↓
Network Trace Analysis
```

---

## 🔎 What Each Tool Helps Identify

| Tool | Purpose |
|--------|----------|
| 🛡️ Azure Network Watcher - IP Flow Verify | Determines whether an NSG allows or denies traffic |
| 🔎 Azure Network Watcher - Connection Troubleshoot | Validates end-to-end reachability between the VMs |
| 💻 Guest OS Checks | Identifies Windows Firewall, Linux Firewall, or service issues |
| 📦 NetMon / netsh / tcpdump | Confirms whether packets reach the destination and receive a response |

> 💡 **Golden Rule:** If **IP Flow Verify = Allow** and **Connection Troubleshoot = Reachable**, Azure networking is likely working correctly and the investigation should move to the guest OS or application layer.