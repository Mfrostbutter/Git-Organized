# GitHub Copilot Instructions — Git-Organized (McNasty)

## Context

You are assisting Michael Frostbutter on McNasty (Windows workstation). Git-Organized is a template and agent system for AI-assisted infrastructure documentation. It is not an infrastructure repo — it's tooling used to bootstrap infrastructure repos.

## Key Rules

- Templates use `{{VARIABLE_NAME}}` placeholders — keep them generic
- Agent prompts in `Agents/` should be environment-agnostic
- Discovery docs provide exact commands with paste-output-here sections
- No real credentials or IPs in templates
- Never delete files — move to `Archive/`

## Structure

- `Templates/` — repo scaffolding (placeholders, not real values)
- `Discovery/` — infrastructure scan walkthroughs
- `Agents/` — Claude Desktop project prompts
- `Scripts/` — shell/Python automation
- `Docs/` — system docs (not infrastructure docs)
