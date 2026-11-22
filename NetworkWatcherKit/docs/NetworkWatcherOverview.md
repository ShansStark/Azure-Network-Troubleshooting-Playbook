# Azure Network Watcher Overview

A short reference for the tools used in this kit and when to reach for each one.

## What Network Watcher is

Azure Network Watcher is a regional service for **monitoring, diagnosing and gaining visibility into** the network resources of your Azure subscription. It is enabled per region (normally automatically when you create a virtual network) and its resource lives in a resource group called `NetworkWatcherRG` by default.

## Tool selection cheat sheet

| Question | Tool | Used in |
|----------|------|---------|
| Can VM A reach destination X, and where does it break? | **Connection Troubleshoot** | 01, 02, 03, 04, 05, 06 |
| Where does Azure send traffic next? | **Next Hop** | 01, 03, 04 |
| Does the NSG allow this specific flow? | **IP Flow Verify** | 01, 02, 05 |
| What NSG rules effectively apply to this NIC? | **Effective security rules** (NIC) | 01, 05 |
| What routes effectively apply to this NIC? | **Effective routes** (NIC) | 01, 03 |
| Which resources are connected and how? | **Topology** | 01, 03, 06 |
| Do packets actually arrive at / leave the VM? | **Packet Capture** | 02, 05, 06 |
| Is the link degrading over time (loss/latency)? | **Connection Monitor** | future scenarios |
| Who is talking to my resources? | **Flow logs / Traffic Analytics** | future scenarios |
| Is my VPN gateway/connection healthy? | **VPN Troubleshoot** | future scenarios |

## What Network Watcher does *not* tell you

- **Guest OS state.** IP Flow Verify and NSG tools cannot see Windows Firewall, `iptables`, or whether a service is listening.
- **Azure Firewall decisions.** IP Flow Verify evaluates NSGs only. Use Azure Firewall logs for firewall rule hits.
- **DNS configuration.** There is no DNS diagnostic tool. Use `nslookup`, `dig` or `Resolve-DnsName` from the VM.
- **Application-layer problems** such as authentication, TLS or database configuration.

## Investigation mindset

1. **Reproduce** from inside the VM so you know the symptom is real.
2. **Split the problem**: routing (Next Hop) vs filtering (IP Flow Verify / firewall) vs the guest (packet capture + OS checks).
3. **Check both directions.** Many "it works one way" cases are return-path problems.
4. **Record evidence**: tool, command, result, rule name or IP. This is what turns a fix into a useful write-up.

## Prerequisites and permissions

- Network Watcher must exist in the VM's region (see [`enable-network-watcher.ps1`](../scripts/enable-network-watcher.ps1)).
- Tools that run from a VM (Connection Troubleshoot with a VM source, Packet Capture, Connection Monitor) need the **Network Watcher agent extension** on that VM.
- Use a role that can read network resources and perform Network Watcher actions on them, such as Network Contributor, scoped to the lab resource group.
- Packet Capture needs a storage account (or a local file path in the VM) to save captures.

## Things to verify before you publish

Azure networking features evolve. Check these against current Microsoft documentation:

- **Flow logs:** Microsoft has been moving from **NSG flow logs** to **virtual network flow logs**, with NSG flow logs on a retirement path. Build any flow-log scenario on VNet flow logs and confirm the current dates.
- **Default outbound access:** new virtual networks are moving to private subnets by default, which affects Scenario 01.
- **Tool names and portal layout:** features such as NSG diagnostics and Connection Monitor have changed names and locations over time.
- **PowerShell parameters:** confirm cmdlet parameters with `Get-Help <cmdlet> -Full` in your installed Az version.

## Further reading

- [Azure Network Watcher documentation](https://learn.microsoft.com/azure/network-watcher/)
- [Azure Firewall documentation](https://learn.microsoft.com/azure/firewall/)
- [Private endpoint DNS configuration](https://learn.microsoft.com/azure/private-link/private-endpoint-dns)
