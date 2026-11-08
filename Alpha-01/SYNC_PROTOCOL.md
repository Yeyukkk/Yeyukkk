# Alpha 01 — GPT / Muse / Codex Synchronization Protocol

**Version:** 1.0 · **Created:** 2026-11-08  
**Visibility:** PUBLIC-SAFE coordination policy only.  
**Authoritative evidence:** Git commit + timestamped source receipt, not chat repetition.

## Ownership and review
- **Rainy (Owner):** strategic objectives, credentials, account permissions, capital decisions.
- **Dr.Trading:** cross-project priorities, risk coordination, escalation.
- **Alpha 01 GPT Lead:** fundamental research decisions, thesis review, entry criteria and Director handoff.
- **Muse:** source collection, research register, first-party earnings/SEC evidence, ongoing monitoring.
- **Codex:** approved data integrations, reproducible analyses, test fixes and versioned pull requests.

## Versioned workflow
1. **Discover before writing:** verify repo name, visibility, branch, HEAD, and existing Issue or document.
2. **Identify one owner:** each task includes `WORKFLOW_ID`, `ISSUE_ID`, `OWNER`, `BASE_COMMIT`, `SOURCE_AS_OF_UTC` and `DECISION_REQUIRED`.
3. **Muse evidence:** attach precise public URLs, publication time, retrieval time, point-in-time availability, data licence, and hashes where appropriate. Do not claim unsourced real-time prices.
4. **Codex engineering:** use an issue-linked branch and PR. Never deploy or merge changes merely because a test passed.
5. **GPT review:** assess evidence quality, conflicts, economic implications, and explicit NEXT_ACTION. Report what was actually read.
6. **Final acknowledgement:** next actor names the exact fetched commit, received files, mismatches and outstanding blockers. No automatic cross-chat memory synchronization is implied.

## Status vocabulary
`PROPOSED`, `SOURCE_VERIFIED`, `OPERATOR_REPORTED`, `INDEPENDENTLY_VERIFIED`, `BLOCKED`, `INCONCLUSIVE`, `REJECTED`, `APPROVED_FOR_RESEARCH`, `APPROVED_FOR_CAPITAL`.

Do not confuse approval for research with permission to place orders.

## File intake
Use `FILE_MANIFEST.csv` to list:
- file path and original source
- content SHA-256
- version / source as-of date
- research stream and evidence level
- classification: `PUBLIC_SAFE`, `PRIVATE_INTERNAL`, `RESTRICTED`, `SECRET_NEVER_UPLOAD`
- current upload status and commit SHA

Do not add a file to the public repository unless it is **PUBLIC_SAFE** and cleared for public distribution. Use private storage/limited-access repository for internal documents and rights-restricted datasets.

## Cross-project boundaries
Alpha 01 is long-horizon U.S. equity value/growth research; it does not inherit Alpha 03 derivatives permissions or Alpha 02 options budgets. Combine issuer exposure only in a Director-level cross-project risk report; do not merge account ledgers.

## Standard handoff
```text
PROJECT: ALPHA01
WORKFLOW_ID:
ISSUE_ID:
OWNER:
BASE_COMMIT:
SOURCE_AS_OF_UTC:
FILES_READ_AND_SHA256:
NEW_EVIDENCE:
STATE_CHANGES:
CONFLICTS_OR_UNKNOWN:
DECISION_REQUIRED:
NEXT_OWNER_AND_ACTION:
```

## What this does NOT provide
GitHub stores versioned artifacts; it does not continuously synchronize local Mac files, Muse's VM state, trading accounts, production processes, or other ChatGPT conversations. Those integrations must be individually connected and tested.
