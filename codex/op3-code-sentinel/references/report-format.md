# Report Format Reference

Use this reference when producing the final answer for debug sessions, code reviews, and audits.

## Review Output

Lead with findings. Keep summaries secondary.

```text
Verdict: APPROVE | COMMENT | REQUEST_CHANGES | NEEDS_INFO

Findings:

[P1] Short actionable title
File: path/to/file.ext:123
Proof: specific code, trace, or command output
Scenario: how the bug or flaw is reachable
Impact: concrete consequence
Fix: minimal recommended change
Verification: test or command to confirm the fix

Checks run:
- Command: ...
  Result: ...

Checks not run:
- Command: ...
  Reason: ...

Residual risk:
- ...
```

If no blocking issues:

```text
Verdict: APPROVE

No blocking issues found.

Checks run:
- ...

Residual risk:
- ...
```

## Debug Output

```text
Diagnostic state: reproduced | partially reproduced | not reproduced

Symptom:
- ...

Evidence:
- ...

Hypothesis:
- ...

Next check:
- ...

Fix:
- ...

Verification:
- ...

Residual risk:
- ...
```

## Good Finding

```text
[P1] User-controlled archive paths can overwrite files outside the extract dir
File: app/uploads.py:88
Proof: `member.filename` is joined directly to `dest` without normalizing and checking the final path remains under `dest`.
Scenario: An uploaded archive containing `../../.ssh/authorized_keys` writes outside the upload directory.
Impact: Arbitrary file overwrite under the service account.
Fix: Resolve the final path, require it stays under `dest`, and reject absolute or parent paths.
Verification: Add a traversal archive test and run `pytest app/test_uploads.py -q`.
```

## Bad Finding

```text
[P1] Possible path traversal
File: app/uploads.py
Problem: Path traversal can happen.
```

Why bad: no line, no source, no sink, no scenario, no impact, no verification.
