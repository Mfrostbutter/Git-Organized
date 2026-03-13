# Proxmox Discovery — PVE Host, Container, and VM Inventory

> **What is this?** A walkthrough for fully documenting a Proxmox VE environment. Run the commands, paste the output, and the result is a complete inventory of your hypervisor, LXC containers, and VMs.
>
> **Time estimate:** 30-60 minutes for a single node. Add 15 min per additional node.
>
> **Prerequisites:** SSH access to your Proxmox host(s) as root.

---

## Section 1 — Proxmox Host Specs

SSH into each PVE host and run these commands. Paste each output below.

```bash
ssh root@{{PVE_HOST_IP}}
```

### CPU

```bash
lscpu | grep -E "^(Architecture|CPU\(s\)|Model name|Thread|Core|Socket)"
```

```
(paste output)
```

### Memory

```bash
free -h
```

```
(paste output)
```

### Disks

```bash
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT,MODEL
```

```
(paste output)
```

### Network Interfaces

```bash
ip -br addr show
```

```
(paste output)
```

### Proxmox Storage Pools

```bash
pvesm status
```

```
(paste output)
```

### Proxmox Version

```bash
pveversion --verbose
```

```
(paste output)
```

### Running Containers & VMs

```bash
echo "=== LXC Containers ===" && pct list && echo "" && echo "=== Virtual Machines ===" && qm list
```

```
(paste output)
```

**Host: ______ (IP: ______)**

- [ ] CPU specs collected
- [ ] Memory specs collected
- [ ] Disk layout collected
- [ ] Network interfaces collected
- [ ] Storage pools collected
- [ ] Proxmox version collected
- [ ] Container/VM list collected

*(Copy this section for each Proxmox node)*

---

## Section 2 — LXC Container Inventory

For each LXC container listed in Section 1, collect specs. Run from the Proxmox host.

Replace `VMID` with the actual container ID.

```bash
# Container configuration
pct config VMID

# OS information
pct exec VMID -- cat /etc/os-release | grep -E "^(NAME|VERSION)="

# Resource usage + running services
pct exec VMID -- bash -c "echo '=== CPU ===' && nproc && echo '=== Memory ===' && free -h && echo '=== Disk ===' && df -h / && echo '=== Services ===' && systemctl list-units --type=service --state=running --no-pager | head -30"

# Network config
pct exec VMID -- ip -br addr show

# Listening ports
pct exec VMID -- ss -tlnp 2>/dev/null || pct exec VMID -- netstat -tlnp
```

**Container: ______ (VMID: ______)**

```
(paste pct config output)
```

```
(paste resource + services output)
```

```
(paste network + ports output)
```

*(Copy this block for each LXC container)*

- [ ] All LXC containers documented
- [ ] Running services noted for each
- [ ] Listening ports collected

---

## Section 3 — VM Inventory

For each VM listed in Section 1, collect specs.

```bash
# VM configuration
qm config VMID

# If qemu-guest-agent is installed:
qm guest exec VMID -- cat /etc/os-release
qm guest exec VMID -- bash -c "nproc && free -h && df -h / && systemctl list-units --type=service --state=running --no-pager | head -30"
qm guest exec VMID -- ip -br addr show
qm guest exec VMID -- ss -tlnp
```

If the guest agent is not installed, SSH directly:

```bash
ssh {{VM_USER}}@{{VM_IP}}
# Then run: nproc, free -h, df -h /, systemctl list-units --state=running, ip -br addr show, ss -tlnp
```

**VM: ______ (VMID: ______)**

```
(paste qm config output)
```

```
(paste resource + services + ports output)
```

*(Copy this block for each VM)*

- [ ] All VMs documented
- [ ] Running services noted for each
- [ ] Listening ports collected

---

## Section 4 — Cluster Overview (Multi-Node)

If you have a Proxmox cluster:

```bash
# Cluster status
pvecm status

# Node list
pvecm nodes

# High availability groups (if configured)
ha-manager status
```

```
(paste output)
```

- [ ] Cluster status collected
- [ ] Node list documented

---

## Section 5 — Compile the Inventory

Once you've completed the sections above, compile into a formatted inventory.

### Option A — Let AI Format It

Paste all raw output from Sections 1-4 into Claude Desktop:

> "Here's the raw output from my Proxmox discovery. Format this into a structured inventory using the template at `Discovery/output-templates/network-inventory.md.template`. Include: physical hosts table, LXC containers table, VMs table, capacity summary, and action items."

### Option B — Fill the Template Manually

Copy [`Discovery/output-templates/network-inventory.md.template`](output-templates/network-inventory.md.template) to `Docs/Network_Inventory.md` and fill it in.

---

## Section 6 — iDRAC / IPMI (Dell Servers)

If your Proxmox host is running on a Dell server with iDRAC:

```bash
# Find iDRAC IP (from Proxmox host)
ipmitool lan print 1 | grep -E "^(IP Address|MAC Address|Subnet)"
```

Via CLI (if racadm is installed or via SSH to iDRAC):

```bash
racadm getversion
racadm getsensorinfo
racadm raid get pdisks -o
racadm getsel -l 20
```

```
(paste output)
```

Document in the inventory:

| Property | Value |
|----------|-------|
| iDRAC IP | |
| Firmware version | |
| Hardware health | |
| RAID status | |
| Disk health | |

- [ ] iDRAC IP confirmed
- [ ] Firmware version noted
- [ ] Hardware health checked
- [ ] Disk health verified

---

## Completion Checklist

- [ ] Section 1 — Proxmox host specs (all nodes)
- [ ] Section 2 — LXC container inventory
- [ ] Section 3 — VM inventory
- [ ] Section 4 — Cluster status (if applicable)
- [ ] Section 5 — Inventory compiled and committed
- [ ] Section 6 — iDRAC / IPMI (if Dell hardware)

**Once complete, your Proxmox environment is fully documented and versioned.**

---

*Re-run this process after adding containers or VMs, hardware changes, or major Proxmox upgrades. See `Agents/discovery-agent.md` for the AI-guided version.*
