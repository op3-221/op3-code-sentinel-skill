---
name: op3-code-sentinel
description: Use when debugging bugs, crashes, regressions, flaky tests, incidents, or reviewing code/security where fixes and severity calls require evidence.
---

# OP3 Code Sentinel

Use this Codex skill for evidence-first debugging, code review, and security
audits. Do not fix before proving root cause, and do not report security
severity before proving reachability or exploitability.

## When to Use

- Bugs, crashes, hangs, regressions, flaky tests, or production incidents.
- Pull request or repository reviews asking for real bugs or security risks.
- Auth/authz, secrets, injection, unsafe code, races, filesystem, network, or
  dependency-vulnerability work.
- Any request that asks for a severity label before the evidence is established.

## Workflow

1. Capture the exact symptom, review scope, diff, or incident boundary.
2. Inspect entry points, trust boundaries, tests, configs, and assets.
3. Gather logs, stack traces, failing tests, command output, and data/control
   flow evidence.
4. Trace source -> validation -> transformation -> sink, or trigger -> state
   -> failure.
5. Test one hypothesis at a time with the smallest safe check.
6. Fix only after proof; label candidate fixes as provisional.
7. Verify with targeted checks plus one broad check appropriate to the repo.
8. Report checks run, checks not run, residual risk, and any uncertainty.

## First Checks

Prefer existing project scripts. Do not install tools without approval.

```bash
git status --short
git diff --stat
git diff --check
git diff --name-only
rg -n "TODO|FIXME|XXX|HACK|password|secret|token|api[_-]?key|private[_-]?key|BEGIN RSA|BEGIN OPENSSH|DATABASE_URL|JWT|SESSION|DEBUG|shell=True|unsafe|exec\.|Command\(|0\.0\.0\.0|privileged|hostNetwork|docker\.sock"
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

Load only the reference files relevant to the language, risk area, or output
format in the current task.

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

## Pitfalls

| Trap | Counter |
| --- | --- |
| "The fix is obvious." | Still prove the cause. |
| "The scanner says vulnerable." | Check version, usage, reachability, impact. |
| "Internal only." | Internal paths still need guardrails. |
| "Mark all unsafe critical." | Refuse until code proves invariant violation. |
| "Quoted shell vars are enough." | Also check empties, roots, globs, `--`, traps. |
