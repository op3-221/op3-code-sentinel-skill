---
name: op3-code-sentinel
description: Use when debugging bugs, incidents, regressions, flaky tests, or auditing code/security with proof before conclusions.
---

# OP3 Code Sentinel

This is the runtime-neutral OP3 Code Sentinel skill. Adapt the tool names to
your agent runtime: use its normal repository search, file reading, shell/test
execution, and patching capabilities.

## When to Use

- Bugs, crashes, hangs, regressions, flaky tests, or incidents.
- Code reviews that need real bugs and security findings.
- Auth/authz, secrets, injection, unsafe code, races, filesystem, network,
  crypto, dependency vulnerabilities, or production-impacting changes.

## Procedure

1. Capture the exact symptom, review scope, or incident boundary.
2. Inspect diffs, entry points, trust boundaries, tests, configs, and assets.
3. Gather logs, stack traces, failing tests, command output, and data/control
   flow evidence.
4. Trace source -> validation -> transformation -> sink, or trigger -> state
   -> failure.
5. Test one hypothesis at a time with the smallest safe check.
6. Fix only after proof; label candidate fixes as provisional.
7. Verify with targeted checks plus one broad check appropriate to the repo.
8. Report checks run, checks not run, and residual risk.

## First Checks

Prefer existing project scripts. Do not install tools without approval.

```bash
git status --short
git diff --stat
git diff --check
git diff --name-only
rg -n "TODO|FIXME|XXX|HACK|password|secret|token|api[_-]?key|private[_-]?key|BEGIN RSA|BEGIN OPENSSH|DATABASE_URL|JWT|SESSION|DEBUG|shell=True|unsafe|exec\.|Command\(|0\.0\.0\.0|privileged|hostNetwork|docker\.sock"
```

If `rg` is not available, use the runtime's preferred file-search mechanism.

## Security Gate

Do not confirm a security issue from pattern matching alone. Show at least one:

- controlled input reaches a dangerous sink;
- auth/authz/tenant boundary is missing on a reachable path;
- secret is exposed, logged, committed, or passed unsafely;
- vulnerable dependency is present and plausibly reachable;
- unsafe/concurrent/filesystem/network/crypto invariant is violated.

If unclear, write `Hypothesis` and give the verification path. Never print
secrets. `unsafe` without code/invariant proof is not confirmed critical.

## References

- Method, OWASP/CWE/NIST: `references/security-method.md`
- Python: `references/python.md`
- Rust: `references/rust.md`
- Go: `references/go.md`
- Shell: `references/shell.md`
- Output format: `references/report-format.md`
- Pressure scenarios: `references/pressure-scenarios.md`

## Output

Reviews start with:

```text
Verdict: APPROVE | COMMENT | REQUEST_CHANGES | NEEDS_INFO
```

Debugging starts with:

```text
Diagnostic state: reproduced | partially reproduced | not reproduced
Evidence:
Hypothesis:
Next check:
```

Every finding needs file/line, proof, scenario, impact, fix, and verification.
If no blocker is found, say so and list residual risk.
