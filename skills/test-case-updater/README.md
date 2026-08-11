# TestRail Phrase Updater — Reusable Skill

**Version:** 2.0.0  
**Author:** folio-org  
**License:** Apache-2.0

---

## What This Skill Does

Automates bulk phrase replacement in TestRail test cases across **any FOLIO module**:

✅ Find phrases matching a pattern  
✅ Replace with new text  
✅ Optionally add Jira references  
✅ Manage pre-release vs current versions  
✅ Dry-run mode for safety  

---

## Works With

| App | Status | Config File | Instructions |
|-----|--------|-------------|--------------|
| **Bulk Edit** | ✅ Ready | `config-bulk-edit.md` | See file directly |
| **Lists** | ⚠️ Ready to add | Create `config-lists.md` | Copy `config-template.md` + fill IDs |
| **Users** | ⚠️ Ready to add | Create `config-users.md` | Copy `config-template.md` + fill IDs |
| **Data Export** | ⚠️ Ready to add | Create `config-data-export.md` | Copy `config-template.md` + fill IDs |
| **Check-in** | ⚠️ Ready to add | Create `config-checkin.md` | Copy `config-template.md` + fill IDs |
| **Check-out** | ⚠️ Ready to add | Create `config-checkout.md` | Copy `config-template.md` + fill IDs |
| **Agreements** | ⚠️ Ready to add | Create `config-agreements.md` | Copy `config-template.md` + fill IDs |
| **Data Import** | ⚠️ Ready to add | Create `config-dataimport.md` | Copy `config-template.md` + fill IDs |
| **Circulation** | ⚠️ Ready to add | Create `config-circulation.md` | Copy `config-template.md` + fill IDs |
| **Inventory** | ⚠️ Ready to add | Create `config-inventory.md` | Copy `config-template.md` + fill IDs |

---

## Folder Structure

```
test-case-updater/
├── SKILL.md                              # Main instructions (generic for all apps)
├── README.md                             # This file
├── references/
│   ├── config-bulk-edit.md               # ✅ Bulk Edit (complete & proven)
│   └── config-template.md                # Template for creating new app configs
└── examples/
    └── example.md                        # Bulk Edit example run (proven result)
```

**To add your app:** Create `references/config-YOUR-APP.md` using `config-template.md` as a guide.

---

## Getting Started

### For Bulk Edit Users

1. Open `SKILL.md`
2. Reference `references/config-bulk-edit.md` for section IDs
3. Provide search/replace terms and run with `$DryRun = true` first

See `examples/example.md` for a proven run result.

---

### For Other Apps (Lists, Users, etc.)

**Step 1: Create your config file**
- Copy `references/config-template.md`
- Save as `references/config-YOUR-APP.md` (e.g., `config-lists.md`)
- Fill in your TestRail section IDs

**Step 2: Use the skill**
- Open `SKILL.md`
- Reference your new `config-YOUR-APP.md` file
- Follow the "Creating a Config for Your App" section in SKILL.md
- Provide search/replace terms and run with `$DryRun = true` first

---

## Design: Minimal Effort, Maximum Reuse

### For Bulk Edit
- **Effort:** Already done (config file exists)
- **What you do:** Provide search/replace terms + dry-run

### For New Apps
- **Effort:** ~10 minutes (create one config file)
- **What you do:** Find 5 section IDs from TestRail, fill them into a template, done
- **No coding required**

### For Maintenance
- **Effort:** Zero code changes
- **What we do:** Add config files as new apps request them
- **Skill logic stays the same forever**

---

## Key Features

✅ **One algorithm for all modules** — find phrase, replace phrase, manage versions  
✅ **Configuration-driven** — section IDs stored in markdown files, not code  
✅ **Safety built-in** — dry-run mode, duplicate detection, error resilience  
✅ **Proven track record** — 82 test cases updated, 61 pre-release copies created, 0 errors  
✅ **Clear documentation** — generic instructions + app-specific examples  
✅ **Quick to adopt** — new app setup is ~10 minutes  

---

## Version History

**v2.0.0** (Current)
- Generalized for any FOLIO module
- Configuration-driven design
- Bulk Edit config included & tested
- Template provided for new apps

**v1.0.0**
- Bulk Edit only
- Hardcoded section IDs

---

## Contributing a New App Config

When your app wants to use this skill:

1. Find your TestRail section IDs (5 minutes)
2. Create `references/config-YOUR-APP.md` from the template (3 minutes)
3. Submit a PR with the new config file (1 minute)
4. Your app can now use the skill immediately

---

## Questions?

- **How do I get started with Bulk Edit?** → See `SKILL.md`
- **How do I set up a new app?** → See `examples/quick-start-new-app.md`
- **How do I find my TestRail section IDs?** → Log into TestRail, click your sections, note the IDs from the URLs
- **Do I need to code?** → No. Just copy-paste config files and section IDs.
