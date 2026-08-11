# Example Run — Bulk Edit - Items

## Inputs

```
SourceSectionIds: 19563
TargetSectionId: 65470
SearchPattern: \bPreview of record\b
ReplacePattern: Preview of records
RefToAdd: UIBULKED-649
ReleaseThreshold: 20
CopyTargetRelease: null
DryRun: false
```

## What Happened

1. Fetched 104 cases from section 19563
2. Found 82 matching `Preview of record` (singular), 19 with only `Preview of records`, 3 with neither
3. Fetched 472 existing titles from target section 65470
4. Determined: 61 pre-release (release < 20), 21 current (release >= 20)
5. Copied 61 pre-release cases to 65470 → C1395081–C1395141
6. Updated all 82 originals: phrase replaced, ref appended
7. No errors

## Log Snippet

```
[2026-06-30 14:22:15] Starting run
[2026-06-30 14:22:15] Source sections: 19563
[2026-06-30 14:22:15] Target section: 65470
[2026-06-30 14:22:16] Matched 82 cases
[2026-06-30 14:22:16] Pre-release: 61 | Current: 21
[2026-06-30 14:22:16] Pre-existing in target: 0
[2026-06-30 14:22:50] Copies created: 61 (C1395081–C1395141)
[2026-06-30 14:23:10] Updates applied: 82
[2026-06-30 14:23:10] Errors: 0
[2026-06-30 14:23:10] Run complete
```

## Re-Running

If the same sections are run again with the same inputs:
- Cases already having `UIBULKED-649` in refs will skip ref addition
- Cases with titles already in the target section will skip copy
- Phrase replacement will be a no-op if already replaced
