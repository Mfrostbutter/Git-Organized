# Change Management Protocol

> **Standard workflow for all infrastructure modifications in a Git-Organized repo.**
> Follow this process for any file edits, config changes, or infrastructure modifications.

---

## The Seven Steps

Every change — big or small — follows this sequence:

### 1. Duplicate
Before touching the original file, note its current state. For file-based changes, copy the original to a working draft. For infrastructure changes, document the current state in the change log before touching anything.

```bash
# For file changes: create a working copy
cp Docs/Network_Inventory.md Docs/Network_Inventory_draft.md

# Or simply note: "current state is X" before changing to Y
```

### 2. Modify
Make your changes to the draft or to the infrastructure. Never modify the last-known-good version directly until you've reviewed the changes.

### 3. Evaluate
Review the modifications before applying them:
- Are the changes correct and complete?
- Do they affect anything downstream (docs, scripts, other configs)?
- Are there any cross-references that need updating?
- Is there a rollback path if something goes wrong?

### 4. Confirm
Get explicit acknowledgment before proceeding:
- For solo work: pause and re-read the change with fresh eyes
- For team work: communicate the change to anyone affected
- For high-risk changes: test in a non-production environment first

### 5. Archive
Move the original to `Archive/` before replacing it. Never delete — the history is valuable.

```bash
# Move original to Archive (use a dated name)
mv Docs/Network_Inventory.md Archive/Network_Inventory_2026-03-01.md

# Now replace with the updated version
mv Docs/Network_Inventory_draft.md Docs/Network_Inventory.md
```

> **Note:** For infrastructure changes (not file changes), the "archive" step is the commit history. Make sure you commit before applying infrastructure changes so you have a snapshot to roll back to.

### 6. Implement
Apply the change — replace the file, push the config, apply the update.

```bash
git add -A
git commit -m "Update: [describe what changed and why]"
git push
```

### 7. Update
Update all related documentation and cross-references:
- Update any docs that reference the changed thing
- Add a change log entry in `Operations/Change-Log/`
- Update the relevant inventory or config doc
- Commit the documentation updates

---

## Change Log Entry Format

For every infrastructure change, create a file in `Operations/Change-Log/`:

**Filename:** `Operations/Change-Log/YYYY-MM-DD_Brief-Description.md`

**Content:**

```markdown
# Change: {{BRIEF_DESCRIPTION}}

**Date:** YYYY-MM-DD
**Engineer:** {{YOUR_NAME}}
**Type:** {{Hardware | Network | Software | Configuration | Documentation}}
**Risk:** {{Low | Medium | High}}

## Summary

What changed and why.

## Details

Specific changes made:
- Item 1
- Item 2

## Impact

What this affects:
- Service/system affected
- Downtime (if any): X minutes
- Other systems affected

## Rollback Plan

How to undo this change if needed:
1. Step 1
2. Step 2

## Verification

- [ ] Change applied successfully
- [ ] Docs updated
- [ ] Tested/verified
```

---

## When to Use This

| Change Type | Full Protocol | Abbreviated |
|-------------|--------------|-------------|
| Production infrastructure change | ✅ Full 7 steps | |
| Config file edit | ✅ Full 7 steps | |
| Documentation update | | ✅ Just commit |
| Adding new files | | ✅ Just commit |
| Emergency fix under pressure | | ✅ Fix → document immediately after |

---

## Emergency Override

In a live incident, do what you need to do. But immediately after the crisis is resolved:

1. Write an incident report in `Operations/Incidents/`
2. Write a change log entry for every change you made
3. Update any affected docs
4. Commit everything with a clear message

The habit is what makes this work. Skipping documentation during an incident is fine — skipping it *after* is what kills institutional knowledge.
