# Workstation Discovery — Local Machine Spec Collection

> **What is this?** A walkthrough for documenting a workstation's hardware, software, and services. Useful for GPU workstations, development machines, or any local host that runs services.

---

## macOS

```bash
# Hardware overview
system_profiler SPHardwareDataType

# CPU info
sysctl -n machdep.cpu.brand_string
sysctl -n hw.logicalcpu hw.physicalcpu

# Memory
sysctl -n hw.memsize | awk '{print $1/1073741824 " GB"}'

# Disk
df -h

# GPU
system_profiler SPDisplaysDataType | grep -E "(Chipset|VRAM|Metal)"

# Network interfaces
ifconfig | grep -E "^(en|utun|lo)" | awk '{print $1}'
ipconfig getifaddr en0

# Listening services
sudo lsof -i -n -P | grep LISTEN
```

---

## Windows (PowerShell)

```powershell
# System info
Get-ComputerInfo | Select-Object CsName, CsProcessors, CsTotalPhysicalMemory, OsName, OsVersion

# CPU
(Get-WmiObject Win32_Processor).Name

# Memory (GB)
[math]::Round((Get-WmiObject Win32_ComputerSystem).TotalPhysicalMemory / 1GB, 1)

# Disk
Get-PSDrive -PSProvider FileSystem | Select-Object Name, @{n='Used(GB)';e={[math]::Round($_.Used/1GB,1)}}, @{n='Free(GB)';e={[math]::Round($_.Free/1GB,1)}}

# GPU
(Get-WmiObject Win32_VideoController).Name

# GPU VRAM
(Get-WmiObject Win32_VideoController).AdapterRAM | ForEach-Object { [math]::Round($_ / 1GB, 1) }

# Listening ports
Get-NetTCPConnection -State Listen | Select-Object LocalAddress, LocalPort, OwningProcess | Sort-Object LocalPort

# Installed software (heavy — optional)
Get-WmiObject Win32_InstalledWin32Program | Select-Object Name, Version | Sort-Object Name
```

---

## Linux

```bash
# CPU
lscpu | grep -E "^(Architecture|CPU\(s\)|Model name|Thread|Core|Socket)"

# Memory
free -h

# Disk
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT,MODEL
df -h

# GPU (NVIDIA)
nvidia-smi

# GPU (AMD)
rocm-smi

# Network
ip -br addr show

# Listening ports
ss -tlnp
```

---

## Document Template

Fill in after running the commands above:

```markdown
# Workstation: {{HOSTNAME}}

**Last documented:** YYYY-MM-DD
**OS:**
**Role:**

## Hardware

| Component | Details |
|-----------|---------|
| CPU | |
| RAM | |
| GPU | |
| Storage | |
| Network | |

## Services

| Service | Port | Notes |
|---------|------|-------|
| | | |

## Scheduled Tasks

| Task | Schedule | Notes |
|------|----------|-------|
| | | |

## Notes

(anything unusual or important to document)
```

---

*Add the completed document to `Docs/` in your infrastructure repo.*
