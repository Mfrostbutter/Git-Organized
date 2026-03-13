# Workflows — How the Git-Organized System Works

---

## The Four Flows

Git-Organized has four distinct workflows depending on what you're doing:

1. **Setup flow** — First-time environment setup (run once)
2. **Discovery flow** — Network and infrastructure scanning (run periodically)
3. **Ongoing maintenance flow** — Daily/weekly habits (run continuously)
4. **Multi-site sync flow** — Comparing parallel environments (run when needed)

---

## 1. Setup Flow

**When:** First time setting up a new environment repo.

**Duration:** 2-4 hours for a typical small environment (1-3 hosts, <10 services).

**Steps:**

```
1. Prerequisites check
   └── Git, editor, SSH key, GitHub account

2. Scaffold the repo
   ├── Create GitHub repo
   ├── Clone locally
   ├── Create directory structure
   └── Generate customized files from templates

3. Sort existing files
   └── Move scattered files into the right folders

4. Environment discovery
   ├── Ping sweep → find every device
   ├── Port scan → find every service
   ├── Spec collection → hardware details
   └── Compile into Docs/Network_Inventory.md

5. First commit
   └── git add -A && git commit -m "Initial setup" && git push

6. Establish habits
   └── Daily workflow, change management protocol
```

**Agent:** Use [`Agents/setup-agent.md`](../Agents/setup-agent.md) for AI-guided walkthrough.

---

## 2. Discovery Flow

**When:** Periodic re-scans, after adding hardware, before/after migrations.

**Duration:** 30-60 minutes for a typical small environment.

**Steps:**

```
1. Load existing inventory
   └── Read Docs/Network_Inventory.md

2. Run scans
   ├── Ping sweep (what's on the network)
   ├── Port scan (what services are running)
   └── Spec collection (version updates, config changes)

3. Compare against existing inventory
   ├── New hosts → identify and document
   ├── Missing hosts → confirm gone or investigate
   └── Version changes → note and flag if security-relevant

4. Update inventory
   └── Edit Docs/Network_Inventory.md

5. Write change log entry
   └── Operations/Change-Log/YYYY-MM-DD_Discovery-Update.md

6. Commit
   └── git add -A && git commit -m "Update inventory: ..." && git push
```

**Agent:** Use [`Agents/discovery-agent.md`](../Agents/discovery-agent.md) for AI-guided walkthrough.

**Triggers for re-discovery:**
- Quarterly (minimum)
- After adding a host, container, or VM
- After replacing network hardware
- Before a migration
- After an incident involving an unknown device

---

## 3. Ongoing Maintenance Flow

**When:** Daily / weekly / after every significant event.

**Duration:** 2-5 minutes for routine commits; 15-30 minutes for incident docs.

### Daily Start

```bash
git pull    # Get teammates' changes
```

### After Any Change

```bash
git add -A
git commit -m "Brief description of what changed and why"
git push
```

### After an Incident

1. Create `Operations/Incidents/YYYY-MM-DD_Brief-Description.md`
2. Include: what happened, impact, timeline, root cause, resolution, prevention
3. Commit and push

### After Infrastructure Changes

1. Follow change management protocol: Duplicate → Modify → Evaluate → Confirm → Archive → Implement → Update
2. Create `Operations/Change-Log/YYYY-MM-DD_Brief-Description.md`
3. Update any affected docs in `Docs/`
4. Commit and push

### Weekly

- Export new/updated workflow files to `Workflows/Exports/`
- `git pull` to get any updates
- Commit and push accumulated changes

---

## 4. Multi-Site Sync Flow

**When:** Running parallel environments (home lab + datacenter, prod + DR, primary + secondary site).

**Duration:** 30-60 minutes.

**Steps:**

```
1. Load both inventories
   ├── Site A: Docs/Network_Inventory.md
   └── Site B: [path to other environment's inventory]

2. Compare systematically
   ├── Service inventory (what runs where)
   ├── Version matrix (same services, different versions)
   ├── Resource allocation (vCPU, RAM differences)
   └── Network layout (subnets, VLANs, firewall rules)

3. Categorize differences
   ├── Intentional (different IPs by design)
   ├── Acceptable (minor allocation variance)
   ├── Needs attention (service version lag)
   └── Critical (missing DR service, security gap)

4. Generate sync report
   └── Outputs/Sync-Report-YYYY-MM-DD.md

5. Add action items
   └── To primary environment's inventory

6. Commit
   └── git commit -m "Add sync report: site A vs site B"
```

**Agent:** Use [`Agents/sync-agent.md`](../Agents/sync-agent.md) for AI-guided walkthrough.

---

## Change Management Protocol

Every modification to existing files follows this sequence. No shortcuts.

| Step | Action | Why |
|------|--------|-----|
| 1. Duplicate | Copy the original before editing | Preserve the known-good state |
| 2. Modify | Edit the copy | Never edit the original directly |
| 3. Evaluate | Review changes before applying | Catch errors before they're live |
| 4. Confirm | Get acknowledgment (from self or teammate) | Prevents rushed changes |
| 5. Archive | Move original to `Archive/` | Never lose history |
| 6. Implement | Replace original with approved version | Apply the change |
| 7. Update | Update cross-references and change log | Keep documentation consistent |

Full protocol: [`Templates/change-management.md`](../Templates/change-management.md)

---

## Git Workflow Reference

### Commands You'll Actually Use

```bash
git status              # What's changed since last commit?
git add -A              # Stage all changes
git commit -m "message" # Commit with description
git push                # Send to GitHub
git pull                # Get teammates' changes
git log --oneline -10   # Last 10 commits
git diff                # See unstaged changes
```

### Good Commit Messages

A good commit message answers: **what changed and why?**

```
# Pattern: [Type]: [what] — [why if not obvious]

Update Network_Inventory: add LXC 207 after DR restore
Add SOP: Proxmox vzdump backup procedure
Fix CLAUDE.md: update MCP server port from 8081 to 8083
Archive old firewall config: replaced SonicWall with FortiGate
```

### File Naming Convention

```
YYYY-MM-DD_Brief-Description.md     # Dated files (incidents, changes)
Procedure_Brief-Description.md      # Procedures/SOPs
Client-Name_Topic.md                # Client notes
Description.md                      # General docs
workflow-name_vN.json               # Workflow exports (versioned)
```
