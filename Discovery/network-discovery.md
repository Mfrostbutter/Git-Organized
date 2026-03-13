# Network Discovery — nmap + SNMP Walkthrough

> **What is this?** An interactive walkthrough for discovering and documenting every device on your network. Run the scans, paste the output, and the result is a formatted network inventory.
>
> **Time estimate:** 30-60 minutes.
>
> **Prerequisites:** nmap installed on your workstation or a host on the network. SNMP community strings for managed switches (often "public" on older devices).

---

## Step 1 — Ping Sweep (Find Every Device)

Discover every responding host on your subnet(s).

```bash
# Replace with your actual subnet
nmap -sn {{PRIMARY_SUBNET}} -oG - | grep "Up" | awk '{print $2, $3}'
```

If you have multiple subnets (e.g., management + storage + IoT):
```bash
nmap -sn {{SUBNET_1}} {{SUBNET_2}} -oG - | grep "Up" | awk '{print $2, $3}'
```

Paste output here:
```
(paste results)
```

Note any unexpected hosts. For each unknown IP, note the MAC address and vendor:
```bash
# Get MAC and vendor for a specific IP (run from same subnet)
nmap -sn {{IP}} --script=arp-scan | grep -E "MAC|Vendor"
# Or just: ping the host, then check ARP table:
arp -n | grep {{IP}}
```

- [ ] Primary subnet scanned
- [ ] Additional subnets scanned (if any)
- [ ] Unexpected hosts noted

---

## Step 2 — Port & Service Scan

For each discovered host, scan for open ports and service versions.

```bash
# Full port scan with service detection (slower but comprehensive)
nmap -sV -O --top-ports 1000 {{PRIMARY_SUBNET}} -oN network-scan.txt

# Quick scan of common ports only (faster)
nmap --top-ports 100 {{PRIMARY_SUBNET}} -oN network-scan-quick.txt
```

> Save output files to `Docs/` with a dated name: `nmap-scan-YYYY-MM-DD.txt`

Interesting findings from port scan:
```
(note unexpected open ports, unusual services, version info)
```

- [ ] Port scan complete
- [ ] Results saved to `Docs/`
- [ ] Unexpected open ports noted

---

## Step 3 — SNMP Discovery (Managed Devices)

Scan for SNMP-enabled devices (switches, routers, APs, UPS, PDUs).

```bash
# Find SNMP-enabled devices
nmap -sU -p 161 {{PRIMARY_SUBNET}} --open -oG - | grep "open"
```

Paste output:
```
(paste results)
```

For each device that responds to SNMP, collect basic info:

```bash
# System description (hardware model, firmware version)
snmpwalk -v2c -c {{SNMP_COMMUNITY}} {{DEVICE_IP}} 1.3.6.1.2.1.1.1.0

# System name (hostname)
snmpwalk -v2c -c {{SNMP_COMMUNITY}} {{DEVICE_IP}} 1.3.6.1.2.1.1.5.0

# System uptime
snmpwalk -v2c -c {{SNMP_COMMUNITY}} {{DEVICE_IP}} 1.3.6.1.2.1.1.3.0

# Interface list and status
snmpwalk -v2c -c {{SNMP_COMMUNITY}} {{DEVICE_IP}} 1.3.6.1.2.1.2.2.1.2
```

**Device: ______ (IP: ______)**
```
(paste SNMP output)
```

*(Copy this block for each SNMP device)*

- [ ] SNMP scan complete
- [ ] Managed switches documented
- [ ] APs documented
- [ ] UPS/PDU documented (if any)

---

## Step 4 — Switch Configuration

Pull running configuration from managed switches.

### UniFi Switches

```bash
# SSH into UniFi switch
ssh admin@{{SWITCH_IP}}

# Show running config
cat /tmp/system.cfg

# Show port status
swctrl port show

# Show VLAN config
swctrl vlan show
```

### Cisco / Generic Managed Switches

```bash
ssh admin@{{SWITCH_IP}}
show running-config
show interfaces status
show vlan brief
show mac address-table
```

### FortiSwitch

```bash
ssh admin@{{SWITCH_IP}}
get system status
show switch trunk
get switch vlan list
```

**Switch: ______ (IP: ______)**
```
(paste config output)
```

- [ ] All managed switch configs collected
- [ ] VLAN assignments documented
- [ ] Port assignments noted

---

## Step 5 — Firewall / Gateway Rules

Document the firewall/gateway rules.

### If Web UI Available

Export the firewall rules or take screenshots and save to `Docs/Firewall-Rules-YYYY-MM-DD.md`.

### If CLI Available

```bash
# FortiGate
show firewall policy

# pfSense / OPNsense
pfctl -sr

# Cisco ASA
show access-list

# UniFi Gateway
# Settings → Firewall & Security → Export (web UI)
```

### Document

| Rule # | Source | Destination | Port/Protocol | Action | Notes |
|--------|--------|-------------|---------------|--------|-------|
| 1 | | | | | |

- [ ] Firewall rules documented
- [ ] NAT rules documented (if any)
- [ ] VPN config documented (if any)
- [ ] Known blocks noted

---

## Step 6 — Compile the Inventory

Once you've completed the sections above, compile into a formatted inventory document.

### Option A — Let AI Format It

Paste all the raw output from Steps 1-5 into Claude Desktop and ask:

> "Here's the raw output from my network discovery scan. Format this into a Network Hardware Inventory document. Use the output template from `Discovery/output-templates/network-inventory.md.template`. Identify any issues or action items."

### Option B — Fill the Template Manually

Copy [`Discovery/output-templates/network-inventory.md.template`](output-templates/network-inventory.md.template) to `Docs/Network_Inventory.md` and fill it in.

- [ ] `Docs/Network_Inventory.md` created
- [ ] Reviewed for accuracy
- [ ] Committed and pushed

---

## Completion Checklist

- [ ] Step 1 — Ping sweep complete
- [ ] Step 2 — Port & service scan complete
- [ ] Step 3 — SNMP discovery complete
- [ ] Step 4 — Switch configs collected
- [ ] Step 5 — Firewall rules documented
- [ ] Step 6 — Inventory compiled and committed

**Once complete, your network is fully documented and versioned.** Re-run this process anytime the environment changes.

---

*This discovery can be re-run anytime. Run quarterly or after any significant network change. See `Agents/discovery-agent.md` for the AI-guided version of this process.*
