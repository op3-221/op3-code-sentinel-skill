---
name: op3-code-sentinel
description: Use when debugging bugs, crashes, regressions, flaky tests, incidents, or auditing code/security for Python, Rust, Go, Shell, infra scripts, auth/authz, secrets, injections, races, unsafe code, dependency vulns, or prod-impacting changes.
---

# OP3 Code Sentinel

## Core Rule

Prove root cause before fixing; reachability or exploitability before reporting security. Do not accept requested severity without evidence.

## Pick The Mode

| Task | Mode | Lead with |
| --- | --- | --- |
| Bug, crash, flaky test, regression, hang | Debug | reproduction state and evidence |
| PR/repo/security review, "find bugs" | Audit | verdict and findings |
| Secret leak, exploitation, prod incident | Incident | evidence preservation |

## Workflow

1. Capture exact symptom or review scope.
2. Inspect diff, entry points, trust boundaries, tests, configs, assets.
3. Gather logs, stack traces, failing tests, tool output, data/control flow.
4. Trace source -> validation -> transformation -> sink, or trigger -> state -> failure.
5. Test one hypothesis at a time with the smallest safe check.
6. Fix after proof; label candidate fixes as provisional.
7. Verify with targeted tests plus a small broad check.
8. Report checks run, checks not run, and residual risk.

## Security Gate

Do not confirm a security issue from pattern-matching alone. Show at least one:

- controlled input reaches a dangerous sink;
- auth/authz/tenant boundary is missing;
- secret is exposed, logged, committed, or passed unsafely;
- vulnerable dependency is present and plausibly reachable;
- unsafe/concurrent/filesystem/network/crypto invariant is violated.

If unclear, write `Hypothesis` and give the verification path. Never print secrets. `unsafe` without code/invariant proof is not confirmed critical.

## References

- Method, OWASP/CWE/NIST: `references/security-method.md`
- Python: `references/python.md`
- Rust: `references/rust.md`
- Go: `references/go.md`
- Shell: `references/shell.md`
- Output format: `references/report-format.md`
- Skill pressure tests: `references/pressure-scenarios.md`

## First Checks

Prefer existing project scripts. Do not install tools without approval.

```bash
git status --short
git diff --stat
git diff --check
git diff --name-only
rg -n "TODO|FIXME|XXX|HACK|password|secret|token|api[_-]?key|private[_-]?key|BEGIN RSA|BEGIN OPENSSH|DATABASE_URL|JWT|SESSION|DEBUG|shell=True|unsafe|exec\\.|Command\\(|rm -rf|0\\.0\\.0\\.0|privileged|hostNetwork|docker.sock"
```

## Stop And Reassess

Stop when three hypotheses fail, reproduction is missing, evidence contradicts the theory, severity was requested but not proved, or the fix touches auth, secrets, migrations, deletion, prod infra, or data loss.

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

Every finding needs file/line, proof, scenario, impact, fix, verification. If no blocker is found, say so and list residual risk.

## Rationalization Traps

| Trap | Counter |
| --- | --- |
| "The fix is obvious." | Still prove the cause. |
| "The scanner says vulnerable." | Check version, usage, reachability, impact. |
| "Internal only." | Internal paths still need guardrails. |
| "Mark all unsafe critical." | Refuse until code proves invariant violation. |
| "Quoted shell vars are enough." | Also check empties, roots, globs, `--`, traps. |
