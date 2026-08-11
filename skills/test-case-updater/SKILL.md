---
name: testrail-phrase-updater
description: >-
  Automates bulk phrase replacement in TestRail test cases across any FOLIO module.
  Find and replace phrases, add Jira references, manage pre-release vs current versions.
  Works with Users, Bulk Edit, Data Export, Lists, Check-in, Check-out, Agreements, Data Import, and more.
license: Apache-2.0
metadata:
  author: folio-org
  version: "2.0.0"
---

# TestRail Phrase Updater — For Any FOLIO Module

Automates bulk phrase replacement in TestRail test cases: find cases matching a phrase, replace it, optionally add a Jira ref, and copy pre-release cases to a target folder.

**Works with any FOLIO module** — Bulk Edit, Lists, Users, Data Export, Check-in, Check-out, Agreements, Data Import, Circulation, Inventory, or any other app with TestRail test cases.

---

## Quick Start (3 Steps)

1. **Find your app's config** in `references/config-YOUR-APP.md`
2. **Copy the section IDs** into the placeholders below
3. **Fill in search/replace terms** and run (use `$DryRun = true` first!)

---

## Required Inputs (Copy from Your App's Config)

| Placeholder | Example | Where to get |
|-------------|---------|--------------|
| `$SourceSectionIds` | `19563,19564` | See `references/config-YOUR-APP.md` |
| `$TargetSectionId` | `65470` | See `references/config-YOUR-APP.md` |
| `$SearchPattern` | `\bPreview of record\b` | What phrase to find (regex allowed) |
| `$ReplacePattern` | `Preview of records` | What to replace it with |
| `$RefToAdd` (optional) | `UIBULKED-649` | Jira issue key (leave empty if not needed) |
| `$ReleaseThreshold` | `20` | Release ID cutoff (copy from config) |
| `$CopyTargetRelease` (optional) | `null` | Release to set on copied cases (usually `null` or a release ID) |
| `$DryRun` | `true` | Always use `true` first to preview changes! |

---

## How It Works (Fixed Steps)

1. **Auth** — Read `TESTRAIL_URL`, `TESTRAIL_USER`, `TESTRAIL_API_KEY` from `.env`; verify connectivity
2. **Fetch sections** — Load all cases from `$SourceSectionIds`
3. **Detect matches** — Find cases where title, preconditions, or steps contain `$SearchPattern`
4. **Categorize** — Determine if each case is **pre-release** (release < threshold) or **current** (release >= threshold)
5. **Pre-release cases:**
   - Copy to `$TargetSectionId` with `$CopyTargetRelease` as release
   - Keep original phrasing and Jira refs in the copy
   - Then update the original with new phrase + new ref (if provided)
6. **Current cases:**
   - Update original only (no copy)
7. **Replace phrase** — In title, preconditions, and step content/expected
8. **Add Jira ref** — If `$RefToAdd` provided and not already present
9. **Sanitize** — Remove invalid characters (NBSP, control chars, etc.)
10. **Commit** — Send updates to TestRail (or report only if `$DryRun = true`)
11. **Log** — Produce markdown report with timestamp, counts, errors, and copy IDs

---

## Before You Run

### 1. Find Your App's Config

**For Bulk Edit (already available):**
- See `references/config-bulk-edit.md` — complete and tested

**For other apps (Lists, Users, Data Export, etc.):**
- If your app's config doesn't exist yet, create one using `references/config-template.md` as a guide
- Name it `references/config-YOUR-APP.md` (e.g., `config-lists.md`, `config-users.md`)
- See "Creating a Config for Your App" section below for step-by-step instructions

### 2. Verify Your .env File
Make sure you have TestRail credentials:
```
TESTRAIL_URL=https://your-testrail.com
TESTRAIL_USER=your-email@example.com
TESTRAIL_API_KEY=your-api-key
```

### 3. Always Dry-Run First
```
$DryRun = true
```
This shows what would change without modifying anything.

---

## Example Inputs

### Example 1: Bulk Edit (Using config-bulk-edit.md)
```
$SourceSectionIds: 19563,19564
$TargetSectionId: 65470
$SearchPattern: \bPreview of record\b
$ReplacePattern: Preview of records
$RefToAdd: UIBULKED-649
$ReleaseThreshold: 20
$CopyTargetRelease: null
$DryRun: true
```

**What this does:**
- Finds 82 cases with "Preview of record" (singular)
- Copies 61 pre-release cases to section 65470
- Updates all 82 originals: replaces phrase, adds UIBULKED-649 ref

---

### Regex Pattern Guide

**Important:** `$SearchPattern` supports regex. Test your pattern carefully:

| Pattern | Example | Use case |
|---------|---------|----------|
| Literal text | `Preview of record` | Exact phrase match (simplest) |
| Word boundary | `\bPreview\b` | Match whole word only, not substrings |
| Escaped chars | `\(optional\)` | Match parentheses; escape with `\` |
| Case-sensitive | `Case.*Sensitive` | Regex is case-sensitive by default |

**Tip:** Start with the simplest pattern first (literal text). Only use regex if you need pattern matching. If your pattern is invalid, the skill will report an error with details.

### Example 2: Lists (Using a config-lists.md — after creation)
```
$SourceSectionIds: <IDs from config-lists.md>
$TargetSectionId: 65470
$SearchPattern: export to CSV
$ReplacePattern: export list as CSV
$RefToAdd: UICAL-888
$ReleaseThreshold: 20
$CopyTargetRelease: null
$DryRun: true
```

---

### Example 3: Your App (Template — fill in your values)
```
$SourceSectionIds: <COPY FROM references/config-YOUR-APP.md>
$TargetSectionId: 65470
$SearchPattern: <YOUR SEARCH>
$ReplacePattern: <YOUR REPLACEMENT>
$RefToAdd: <JIRA-KEY or leave empty>
$ReleaseThreshold: 20
$CopyTargetRelease: null
$DryRun: true
```

---

## Creating a Config for Your App

### For New Apps (Lists, Users, Data Export, etc.)

If your app doesn't have a config file yet, follow these steps:

**Step 1:** Copy `references/config-template.md` as the base

**Step 2:** Create a new file: `references/config-YOUR-APP.md`  
Examples:
- `references/config-lists.md`
- `references/config-users.md`
- `references/config-data-export.md`

**Step 3:** Fill in your TestRail information:

```markdown
# YOUR-APP — TestRail Config

## Section IDs

| Section | ID |
|---------|-----|
| Main tests | <FIND-FROM-TESTRAIL> |
| Capabilities | <FIND-FROM-TESTRAIL> |
| Edge cases | <FIND-FROM-TESTRAIL> |

## Target Section

Sunflower/Manual tests: `65470`

## Release IDs

| Release | ID |
|---------|-----|
| Sunflower | 19 |
| Trillium | 20 |
| Umbrealeaf | 21 |

## Project/Suite

TestRail project: `14` (FOLIO Bug Fest)
TestRail suite: `21` (Master)
```

**Step 4:** Find your section IDs:
- Log into TestRail → Your app's project
- Click each section in the left panel → Note the ID from the URL or properties
- Fill in the table above

**Step 5:** You're done! Your app can now use the skill.

### Example: Bulk Edit Config

See `references/config-bulk-edit.md` for a complete example.

---

## Error Resilience

- ✅ If a case already has the phrase replaced → skip (no-op)
- ✅ If a case already has `$RefToAdd` in refs → skip ref addition (but still replace phrase)
- ✅ If pre-release title matches existing target title → skip copy (already duplicated)
- ✅ If API call fails → log error, continue processing remaining cases
- ✅ If copy succeeds but update fails → log copy ID for manual follow-up

**Partial failure handling:** If some cases fail and others succeed:
1. Check the error log for which cases failed
2. Fix the issue (e.g., invalid section ID, authentication, rate limit)
3. Re-run with the same inputs: already-updated cases will be skipped, only failed cases will be retried (safe to re-run)

---

## Dry Run (Recommended First Step)

When `$DryRun = true`:
- Fetch and analyze all cases
- Report which cases match, their bucket (pre-release/current), what would be copied/updated
- **Make ZERO modifications to TestRail**

When `$DryRun = false`:
- Execute all changes
- Create copies as needed
- Update original cases
- Log results

---

## Output

You'll receive a markdown log with:
- Run timestamp and inputs
- Per-case results (copy ID if created, update status)
- Summary counts: matched, pre-release, current, copied, updated, errors
- List of any errors for manual review

---

## Common Questions

**Q: My app doesn't have a config file yet. What do I do?**  
A: Create one! Use the template in "Creating a Config for Your App" section. You only need section IDs (get them from TestRail project settings).

**Q: Can I run this on multiple apps at once?**  
A: No, one run per app. But the inputs are identical, so you can copy-paste and just change the app name.

**Q: What if I get errors?**  
A: Check the error log. Most common: invalid section ID, authentication failure, or TestRail API rate limit. Fix and re-run (cases already updated won't be updated again).

**Q: How do I find my app's section IDs?**  
A: Log into TestRail → Navigate to your app's project → Click the sections in the left panel → Note the IDs in the URL or section list.

---

## Verify Success

After the run:
1. ✅ Check the markdown log for error count (should be 0 for first run in dry-run mode)
2. ✅ If dry-run: review the report, then re-run with `$DryRun = false`
3. ✅ Log into TestRail and spot-check a few updated cases
4. ✅ Verify pre-release cases were copied to the target section
5. ✅ Verify current release cases were updated only (not copied)
