# Git-Organized

**AI-assisted Git onboarding and infrastructure documentation system.**

Git-Organized gives you a repeatable, agent-guided process for turning any environment — datacenter, home lab, cloud, workstation — into a fully documented, version-controlled, AI-searchable repository.

---

## What It Is

Most infrastructure documentation fails because it's manual, bespoke, and immediately out of date. Git-Organized fixes this with:

- **Templates** for repo scaffolding — directories, READMEs, `.gitignore`, `CLAUDE.md`
- **Discovery walkthroughs** for different environment types (datacenter, home lab, Proxmox, cloud, workstation)
- **Agent prompts** that Claude Desktop uses to walk someone through the entire process interactively
- **Scripts** that automate the scan-and-format pipeline (Phase 3)
- **n8n workflows** for scheduled discovery and drift alerting (Phase 4)

> **This is not a Git tutorial.** It's an infrastructure documentation system that uses Git as its backbone.

---

## Quick Start

### Full Setup (Recommended)
Use the Setup Agent to be walked through the entire process interactively:

1. Open Claude Desktop
2. Start a new conversation with the contents of [`Agents/setup-agent.md`](Agents/setup-agent.md) as your project instructions
3. Ask: "Walk me through setting up a Git-Organized repo for my environment"
4. The agent will scaffold the repo, guide you through discovery scans, and help you commit

### Just the Templates
Clone this repo and copy the templates to your new infrastructure repo:

```bash
git clone https://github.com/Mfrostbutter/Git-Organized.git
cp -r Git-Organized/Templates/repo-scaffold/ my-infra-repo/
```

Edit all `{{VARIABLE}}` placeholders with your actual values, then initialize git.

### Just the Discovery
Go straight to the relevant discovery walkthrough for your environment:

- **Proxmox / datacenter:** [`Discovery/proxmox-discovery.md`](Discovery/proxmox-discovery.md)
- **Network devices:** [`Discovery/network-discovery.md`](Discovery/network-discovery.md)
- **Workstation:** [`Discovery/workstation-discovery.md`](Discovery/workstation-discovery.md)

Run the scans, paste output into Claude, and it will format your inventory document.

---

## Directory Structure

```
Git-Organized/
├── README.md                         This file
├── CLAUDE.md                         Agent instructions for this repo
├── .gitignore
│
├── Templates/
│   ├── repo-scaffold/
│   │   ├── README.md.template        Repo README with placeholders
│   │   ├── CLAUDE.md.template        Claude Code instructions template
│   │   ├── .gitignore.template       Standard .gitignore for infra repos
│   │   └── directory-structure.yml   Folder tree + READMEs definition
│   ├── change-management.md          Standard change workflow
│   ├── git-organized.md.template     Interactive onboarding guide (template)
│   └── env-file.template             .env template with common placeholders
│
├── Discovery/
│   ├── network-discovery.md          nmap + SNMP scan walkthrough
│   ├── proxmox-discovery.md          PVE host, container, VM inventory
│   ├── workstation-discovery.md      Local machine spec collection
│   └── output-templates/
│       ├── network-inventory.md.template    Formatted inventory output
│       └── capacity-summary.md.template     Resource utilization summary
│
├── Agents/
│   ├── setup-agent.md               Full environment setup (first run)
│   ├── discovery-agent.md           Network discovery (ongoing re-runs)
│   ├── sync-agent.md                Multi-site drift comparison
│   └── audit-agent.md               Periodic infrastructure audit
│
├── Scripts/                          Phase 3 — shell/Python automation
│   └── README.md
│
├── Docs/
│   ├── philosophy.md                Why AI-documented infrastructure matters
│   └── workflows.md                 Setup, discovery, ongoing, multi-site flows
│
├── n8n/                              Phase 4 — workflow automation
│   └── README.md
│
└── Archive/                          Convention maintained — never delete
```

---

## The Three Paths

| Path | When to use | Where to start |
|------|-------------|----------------|
| **Full agent-guided setup** | New environment, starting fresh | [`Agents/setup-agent.md`](Agents/setup-agent.md) |
| **Template-only** | You know what you're doing, just need the structure | [`Templates/repo-scaffold/`](Templates/repo-scaffold/) |
| **Discovery-only** | Environment already documented, running new scans | [`Discovery/`](Discovery/) |

---

## Real-World Reference Implementations

This system was built from patterns developed in two real environments:

- **Home Lab** — multi-node Proxmox cluster, LXC services, GPU workstation, self-hosted AI platform
- **Datacenter migration** — Dell PowerEdge hardware, Proxmox VE, home-lab-to-DC promotion

Both serve as reference implementations for the templates and discovery walkthroughs. The `Templates/` directory contains generalized versions extracted from those real environments.

---

## Philosophy

Infrastructure documentation fails because:
1. It's written once and immediately becomes stale
2. It lives in someone's head or in one-off text files
3. There's no version history — you can't see what changed or when

Git-Organized solves this by making documentation a living, versioned artifact:
- **Git tracks every change** with timestamps and commit messages
- **AI assists with the writing** — you provide the data, it formats the docs
- **Discovery is repeatable** — re-run the scans when something changes
- **Archive/ pattern** — nothing gets deleted, history is always preserved

> Full philosophy: [`Docs/philosophy.md`](Docs/philosophy.md)

---

## Contributing

Issues and PRs welcome. The templates and agent prompts are the highest-value contributions — real-world testing with different environment types is how they improve.

---

## Companion Repo

[**Git-Organized-AI-Policy**](https://github.com/Mfrostbutter/Git-Organized-AI-Policy) applies the same pattern to AI governance — the scaffold, discovery walkthroughs, agents, and drift-loop wiring an org needs to ship AI policy as code instead of as a PDF.

---

*Built by [Agenius AI Labs](https://ageniuslabs.com). MIT License.*
