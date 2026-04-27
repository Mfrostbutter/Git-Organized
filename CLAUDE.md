> **Developer Knowledge Base — CHECK FIRST:** Before writing code or researching any platform/API, query the `dev_knowledge` Qdrant collection (LXC 209, 10.10.0.60:6333) via the `agenius-dev-mcp` server. It contains chunked, embedded documentation for 29 platforms (~75K vectors) including n8n, Qdrant, Cloudflare, Etsy, Square, Shopify, and more. Filterable by platform, group, doc_type, and topics.

# Git-Organized — Claude Code Instructions

Last updated: 2026-04-03

> This file provides context for Claude Code when working in this repository. Git-Organized is a template and agent system for AI-assisted infrastructure documentation.

## What This Repo Is

Git-Organized contains reusable templates, discovery walkthroughs, and agent prompts for setting up version-controlled, AI-searchable infrastructure documentation. It is not an infrastructure repo itself — it is the tooling used to bootstrap infrastructure repos.

## Repository Structure

```
Git-Organized/
├── Templates/          Repo scaffolding templates (placeholders, not real values)
├── Discovery/          Infrastructure scan walkthroughs and output templates
├── Agents/             Claude Desktop project prompts for guided onboarding
├── Scripts/            Shell/Python automation (Phase 3)
├── Docs/               Philosophy and workflow documentation
├── n8n/                Workflow automation (Phase 4)
└── Archive/            Retired content (never delete — move here)
```

## Working in This Repo

### Templates
All templates use `{{VARIABLE_NAME}}` placeholders. When editing templates:
- Keep placeholders generic — they should work for any environment type
- Add new placeholders rather than hardcoding specific values
- Note placeholder meanings in comments or the `directory-structure.yml`

### Agent Prompts
Agent prompts in `Agents/` are Claude Desktop project instructions. They should:
- Be environment-agnostic (not assume Proxmox, datacenter, etc.)
- Ask clarifying questions before taking action
- Reference the Templates and Discovery docs
- Follow the change management workflow from `Templates/change-management.md`

### Discovery Walkthroughs
Discovery docs in `Discovery/` are interactive scan guides. They should:
- Provide exact commands to run
- Have paste-output-here sections for raw scan data
- Include checkboxes for progress tracking
- End with output template instructions

## Change Management

Before modifying any file in this repo:
1. **Duplicate** — note what you're about to change
2. **Modify** — make the changes
3. **Evaluate** — check for broken cross-references, placeholder consistency
4. **Commit** — use descriptive message: what changed and why

## Coding Conventions (Scripts)

- Shell scripts: bash, POSIX-compatible where possible
- Python scripts: 3.10+, no external dependencies unless clearly documented
- All scripts use flags (`--name`, `--subnet`, etc.) not positional args
- Scripts print usage when called with `--help`
- No hardcoded paths, IPs, or credentials

## Important Notes

- **Never delete files** — move to `Archive/`
- **No real credentials or IPs in templates** — use `{{VARIABLE}}` placeholders
- `Templates/` should always contain generic content, never org-specific values
- The `Docs/` directory is for the Git-Organized system docs, not infrastructure docs
