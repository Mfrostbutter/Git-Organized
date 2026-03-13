# Philosophy — Why AI-Documented Infrastructure Matters

---

## The Problem

Every IT environment has the same disease: **tribal knowledge**.

The person who built the network knows what that mystery device on `.204` is. The person who configured the firewall knows why that rule exists. The person who set up the backup job knows that it only works if the storage pool is mounted first.

When that person is unavailable — sick, on vacation, or gone — the knowledge disappears. The team spends hours reverse-engineering what should have taken minutes to find.

Documentation is the cure. But documentation has its own disease: **it becomes stale immediately**.

You write the network inventory in January. In February someone adds a NAS. In March you replace the firewall. By April the inventory is wrong in three places and nobody trusts it anymore, so they stop updating it, so it gets worse, so they stop reading it.

This is the documentation death spiral. Every team experiences it.

---

## The Git-Organized Solution

Git-Organized breaks the death spiral with three changes:

### 1. Version Control for Documentation

Git gives documentation a change history. Every update is a commit with a timestamp and a message. You can see who changed what, when, and why. You can diff against the previous state.

This makes documentation trustworthy again. When you read the inventory, you can see "last updated 3 days ago" and trust it. When something's wrong, you can find out when it changed.

### 2. AI Assistance for the Writing

The hardest part of documentation isn't knowing the information — it's turning raw data into readable prose. Nobody wants to format a table by hand after running 20 nmap commands.

AI assistants (Claude, Copilot) remove this friction. You paste raw scan output; AI formats the inventory table. You describe an incident verbally; AI writes the incident report. You run the commands; AI compiles the results.

This makes documentation fast enough that people actually do it.

### 3. Discovery as Process, Not Event

Traditional documentation is written once and then forgotten. Git-Organized treats discovery as a repeatable process — something you run quarterly, or after every significant change, or before/after a migration.

The discovery walkthroughs are designed to be re-run. The inventory format is designed to be diffed. The audit agent is designed to tell you what changed since last time.

This makes documentation current by design, not by heroic effort.

---

## The Archive Pattern

One principle above all others: **never delete**.

Every file that gets superseded goes to `Archive/` — not to the trash. The original network inventory from January stays in `Archive/` even after the February update replaces it.

Why? Because you will need it. Not today. But in six months when you're troubleshooting why something worked before and doesn't now. Or when you're trying to understand what the environment looked like before a migration. Or when a client asks what their environment looked like last year.

Git gives you version history, but `Archive/` gives you named snapshots with context. "The pre-migration inventory" is more findable than "the commit from three months ago."

---

## The Commit Message as Documentation

Every `git commit -m "update"` is a missed opportunity.

A good commit message is a sentence or two explaining what changed and why:

```
Update network inventory: add LXC 207 (monitoring) after Proxmox restore

Container 207 restored from vzdump backup taken during DR test. Assigned
static IP 10.10.0.48, port 3000 for Grafana. Replaces old standalone
monitoring server at .35 (now archived).
```

Six months later, you will be grateful for that message. You'll know exactly what happened and why without reading the diff.

---

## AI as Collaborator, Not Authority

One critical principle: AI assists with documentation, but you verify it.

AI can:
- Format raw scan output into inventory tables
- Draft procedures from your verbal description
- Write incident reports from your notes
- Suggest commit messages
- Flag action items from discovery output

AI cannot:
- Know if the documentation is accurate (it trusts what you paste)
- Understand the "why" behind decisions (unless you tell it)
- Catch errors it doesn't have context to recognize

Always review AI-generated documentation before committing. The AI is a very fast, very capable typist — but you are the authority on your infrastructure.

---

## Git-Organized is Infrastructure, Not Git Education

A common misconception: Git-Organized is about teaching people to use Git.

It's not. Git is a tool. Git-Organized is a *system* that uses Git to solve the tribal knowledge problem.

The people using Git-Organized don't need to understand Git object models or rebase strategies. They need to know five commands (`status`, `add`, `commit`, `push`, `pull`) and one mental model (every change is documented before it's made).

The complexity lives in the system design. The day-to-day use is simple by design.

---

## Built From Real Infrastructure

Git-Organized was extracted from a real IT services firm's home lab and datacenter:

- **Home lab:** 3-node Proxmox cluster, 7 LXC containers, RTX 5060 GPU workstation, self-hosted AI knowledge platform
- **Datacenter:** Dell PowerEdge R620, separate Proxmox node, parallel service deployment

The templates and discovery walkthroughs were written and tested against those real environments. They're not theoretical — they reflect what actually needed to be documented and how.

---

*"The best documentation is the documentation that exists." — Not perfect docs written once, but living docs kept current.*
