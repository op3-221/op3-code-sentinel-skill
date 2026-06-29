---
name: op3-code-sentinel
description: Use when debugging bugs, crashes, regressions, flaky tests, incidents, or auditing code/security with evidence-first reasoning.
---

# OP3 Code Sentinel

Use this OpenCode skill for evidence-first debugging, code review, and security
audits. It is written for OpenCode skill locations such as
`.opencode/skills/op3-code-sentinel/` in a project or
`~/.config/opencode/skills/op3-code-sentinel/` globally.

## When to Use

- Bugs, crashes, hangs, regressions, flaky tests, or production incidents.
- Code reviews that need real bug findings, not style-only feedback.
- Security audits involving auth/authz, secrets, injection, unsafe code, races,
  filesystem, network, crypto, or dependency reachability.
- Any request that asks for severity before exploitability is proven.

## Procedure

1. Capture the exact symptom or review scope.
2. Inspect the diff, entry points, trust boundaries, tests, configs, and assets.
3. Use OpenCode's available file search, file read, and shell execution
   capabilities to gather evidence.
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
rg -n "TODO|FIXME|XXX|HACK|password|secret|token|api[_-]?key|private[_-]?key|BEGIN RSA|BEGIN OPENSSH|DATABASE_URL|JWT|SESSION|DEBUG|shell=True|unsafe|exec\.|Command\(|rm -rf|0\.0\.0\.0|privileged|hostNetwork|docker\.sock"
```

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

Read only the reference files relevant to the current language, risk area, or
output format.

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
