# Audit Trail — Hash-Chained Evidence Ledger

## Overview

The audit trail is a deterministic, append-only evidence ledger that records every stage's verification results in a tamper-evident chain. Each entry contains a SHA-256 hash of the previous entry, making the chain cryptographically verifiable. If any entry is modified, the hash chain breaks and the tampering is detectable.

This brings fable mode in line with StepProof, HighHarness, and similar projects that treat auditability as a first-class requirement.

---

## Directory Structure

All audit trail data lives under `.fable/trail/`:

```
.fable/trail/
├── .fableignore          # Patterns to exclude from evidence (secrets, vendor)
├── chain.audit.jsonl     # Append-only hash-chained audit entries
├── evidence/             # Per-stage evidence artifacts
│   ├── stage-001-NdY8/   # Stage-specific evidence directory (random suffix)
│   │   ├── check-output.txt
│   │   ├── check-exit-code.txt
│   │   └── artifacts/
│   └── stage-002-pL3a/
│       ├── check-output.txt
│       ├── check-exit-code.txt
│       └── artifacts/
└── verify.json           # Verification report after Full Proof
```

---

## Audit Entry Format

Each line in `chain.audit.jsonl` is a JSON object. The `this_hash` field is computed over the canonical entry bytes with `this_hash` blanked. The `prev_hash` field matches the `this_hash` of the previous entry (the genesis entry has `prev_hash: "0000000000000000000000000000000000000000"`).

```json
{
  "entry_id": "aud-001",
  "timestamp": "2026-07-17T10:30:00Z",
  "run_id": "run-auth-refactor-7a92",
  "stage_id": "stage-001",
  "stage_name": "Refactor authentication module",
  "action": "verify",
  "status": "PASS",
  "check_command": "npx tsc --noEmit",
  "check_exit_code": 0,
  "evidence_summary": "TypeScript compilation: 0 errors, 0 warnings across 47 files",
  "evidence_refs": [".fable/trail/evidence/stage-001-NdY8/check-output.txt"],
  "planned_files": ["src/auth/login.ts", "src/auth/session.ts"],
  "scope_drift": false,
  "prev_hash": "a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1",
  "this_hash": "f1e2d3c4b5a69788796a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5d4"
}
```

### Field Reference

| Field | Required | Description |
|-------|----------|-------------|
| `entry_id` | yes | Unique identifier (`aud-NNN`) |
| `timestamp` | yes | ISO 8601 timestamp |
| `run_id` | yes | Identifies the fable mode session run |
| `stage_id` | yes | Identifies the stage within the run |
| `stage_name` | yes | Human-readable stage name from the plan |
| `action` | yes | `start`, `verify`, `re-verify`, `complete`, `fail`, `bounce` |
| `status` | no | `PASS`, `FAIL`, `UNVERIFIED`, `IN_PROGRESS`, `BOUNCED` |
| `check_command` | no | The exact verification command executed |
| `check_exit_code` | no | Exit code from the verification check |
| `evidence_summary` | no | One-line summary of the evidence |
| `evidence_refs` | no | Paths to evidence artifacts |
| `planned_files` | no | Files declared in the stage plan for this stage |
| `scope_drift` | no | `true` if files outside `planned_files` were modified |
| `prev_hash` | yes | SHA-256 of the previous entry's canonical bytes |
| `this_hash` | yes | SHA-256 of this entry's canonical bytes (with `this_hash` set to empty string) |

---

## Hash Chaining Protocol

### Canonical Entry Format

To compute `this_hash`:

1. Take the entry JSON and set `this_hash` to `""`.
2. Serialize to canonical JSON (sorted keys, no whitespace).
3. Compute `SHA-256(canonical_json)`.
4. Set `this_hash` to the hex-encoded hash.

### Chain Verification

To verify the entire chain:

```
for each entry in chain.audit.jsonl:
  assert entry.this_hash == SHA-256(canonical(entry with this_hash=""))
  assert entry.prev_hash == previous_entry.this_hash (or "00...0" for genesis)
```

If any assertion fails, the chain has been tampered with.

---

## Audit Trail Lifecycle

### Per-Stage Recording

The Blacksmith records audit entries at each stage boundary:

| Action | When | Entry `action` |
|--------|------|----------------|
| Stage starts | Before execution begins | `start` |
| Check passes | After verification check passes | `verify` or `complete` |
| Check fails | After verification check fails | `fail` |
| Re-verify | After fix and re-run | `re-verify` |
| Bounce | When bouncing to a target | `bounce` |

### Per-Run Recording

The Sage records a verification entry after the Final Proof:

```json
{
  "entry_id": "aud-ver-001",
  "timestamp": "2026-07-17T11:00:00Z",
  "run_id": "run-auth-refactor-7a92",
  "stage_id": "final-proof",
  "stage_name": "Final Proof of Deeds",
  "action": "final_proof",
  "status": "PASS",
  "check_command": "npm run build && npm test",
  "check_exit_code": 0,
  "evidence_summary": "Build succeeds, 142 tests pass, requirements traceability complete",
  "evidence_refs": [".fable/trail/evidence/final-proof/check-output.txt"],
  "planned_files": ["src/auth/", "src/middleware/"],
  "scope_drift": false,
  "prev_hash": "...",
  "this_hash": "..."
}
```

---

## Chain Verification Command

The Blacksmith includes a deterministic verification step to audit the trail:

```bash
# Verify chain integrity
# For each entry, re-compute SHA-256 and compare against stored this_hash
# Then verify prev_hash matches previous entry's this_hash

python3 -c "
import json, hashlib, sys

entries = []
with open('.fable/trail/chain.audit.jsonl') as f:
    for line in f:
        entries.append(json.loads(line.strip()))

prev_hash = '0' * 64
for i, entry in enumerate(entries):
    # Canonicalize
    verify = dict(entry)
    verify['this_hash'] = ''
    canonical = json.dumps(verify, sort_keys=True, separators=(',', ':'))
    computed = hashlib.sha256(canonical.encode()).hexdigest()
    
    if computed != entry['this_hash']:
        print(f'FAIL: Entry {i} hash mismatch')
        sys.exit(1)
    if entry['prev_hash'] != prev_hash:
        print(f'FAIL: Entry {i} prev_hash mismatch')
        sys.exit(1)
    prev_hash = entry['this_hash']

print(f'PASS: {len(entries)} entries, chain intact')
"
```

Exit code 0 means the chain is intact. Non-zero means tampering detected.

---

## Integration with Fable Mode Stages

### Cartographer Integration

When defining the stage plan, the Cartographer includes:

```markdown
### Audit Trail Configuration
- **Trail enabled**: true
- **Evidence directory**: .fable/trail/evidence/
- **Hash chain file**: .fable/trail/chain.audit.jsonl
```

### Blacksmith Integration

After each verification check:

1. Save the check output to `.fable/trail/evidence/stage-N-suffix/check-output.txt`
2. Create an audit entry with the check result
3. Compute the hash chain
4. Append to `chain.audit.jsonl`

### Sage Integration

During Final Proof:

1. Verify the hash chain integrity (run the verification command)
2. Include the result in the Final Proof verdict
3. Record the Final Proof entry in the audit trail

### Mirror Integration

During self-review:

1. Check that every stage has at least one audit entry
2. Verify the hash chain is intact
3. Flag missing or inconsistent audit entries

---

## Anti-Patterns

### Skipping Audit Entries
- Every stage must produce at least one audit entry
- Missing entries = incomplete audit trail
- The Mirror must flag missing entries

### Writing After the Fact
- Audit entries must be written at the time of the action, not retroactively
- A backdated audit trail violates tamper evidence

### Storing Secrets in Evidence
- Evidence artifacts must not contain secrets or credentials
- The `.fableignore` file should list patterns to redact
- Use environment variable references instead of literal values

---

## Example: Complete Run Audit Trail

```
entry_id: aud-001, action: start,     stage: Stage 1, status: IN_PROGRESS
entry_id: aud-002, action: verify,    stage: Stage 1, status: PASS
entry_id: aud-003, action: start,     stage: Stage 2, status: IN_PROGRESS
entry_id: aud-004, action: fail,      stage: Stage 2, status: FAIL
entry_id: aud-005, action: start,     stage: Stage 2 (retry), status: IN_PROGRESS
entry_id: aud-006, action: verify,    stage: Stage 2 (retry), status: PASS
entry_id: aud-007, action: start,     stage: Stage 3, status: IN_PROGRESS
entry_id: aud-008, action: verify,    stage: Stage 3, status: PASS
entry_id: aud-009, action: final_proof, stage: Final Proof, status: PASS
```

Each entry is hash-chained to the previous. Tamper with entry aud-004 and all subsequent hashes break.
