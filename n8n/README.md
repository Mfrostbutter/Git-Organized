# n8n Integration — Phase 4

> **Status:** Planned (Phase 4). This directory will contain n8n workflow JSONs for automated discovery and alerting.

---

## Planned Workflows

### `scheduled-discovery.json`

Runs nmap on a schedule (weekly recommended), commits changes to the inventory if anything changed, sends a notification summary.

**Triggers:**
- Schedule (cron)
- Manual webhook

**Actions:**
1. Run nmap ping sweep on configured subnet(s)
2. Compare against last scan output
3. If changes detected: format diff, update inventory, git commit + push
4. Send notification (Slack, email, webhook) with summary

### `inventory-diff-alert.json`

Monitors for inventory changes and sends alerts when new or missing hosts are detected.

**Triggers:**
- Git webhook (on push to main)

**Actions:**
1. Pull latest inventory
2. Diff against previous version
3. If new unrecognized hosts: send high-priority alert
4. If missing hosts: send warning alert
5. Log to change log

---

## Setup (When Available)

1. Import the workflow JSON via n8n → Settings → Import
2. Configure credentials: Git (SSH key or token), notification service
3. Update the subnet variable in the workflow settings
4. Activate the workflow

---

## Requirements

- n8n self-hosted (any recent version) or n8n Cloud
- Git credentials with push access to your infrastructure repo
- nmap available on the n8n host (or via SSH to a host that has it)
- Notification service credentials (Slack, SMTP, etc.)

---

## Contributing

Submit PRs or open issues at [github.com/Mfrostbutter/Git-Organized](https://github.com/Mfrostbutter/Git-Organized).
