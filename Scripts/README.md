# Scripts — Phase 3

> **Status:** Planned (Phase 3). This directory will contain shell and Python scripts that automate the scan-and-format pipeline.

---

## Planned Scripts

| Script | Purpose | Usage |
|--------|---------|-------|
| `scaffold.sh` | Creates directory structure from `directory-structure.yml` | `./scaffold.sh --name "My-Infra" --type datacenter` |
| `nmap-discovery.sh` | Runs nmap scans and formats output | `./nmap-discovery.sh --subnet 10.0.0.0/24 --output scan-results/` |
| `spec-collector.sh` | SSH into hosts and collect specs (lscpu, free, lsblk) | `./spec-collector.sh --hosts hosts.txt --output specs/` |
| `snmp-collector.sh` | SNMP walk on discovered devices | `./snmp-collector.sh --community public --subnet 10.0.0.0/24` |
| `inventory-formatter.py` | Parses raw scan output into formatted markdown | `python inventory-formatter.py --input specs/ --template network-inventory.md.template` |
| `diff-inventory.py` | Compares two inventory snapshots | `python diff-inventory.py --old inventory-v1.md --new inventory-v2.md` |

---

## Contributing

Scripts should:
- Use flags (`--name`, `--subnet`) not positional arguments
- Print usage when called with `--help`
- Handle errors gracefully with clear messages
- Work without root unless strictly necessary
- Be tested on macOS (zsh) and Linux (bash)

Submit PRs or open issues at [github.com/Mfrostbutter/Git-Organized](https://github.com/Mfrostbutter/Git-Organized).
