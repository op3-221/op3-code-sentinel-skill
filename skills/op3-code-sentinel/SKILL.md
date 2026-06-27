---
name: op3-code-sentinel
description: Debug bugs and audit code security with proof before fixing.
platforms: [linux, macos]
---

# OP3 Code Sentinel Skill

Debug real bugs, crashes, regressions, flaky tests, and security issues in
Python, Rust, Go, and Shell. Forces evidence before fixes and before security
findings — no pattern-matching verdicts, no accepted severity without proof.

## When to Use

- Debugging crashes, regressions, flaky tests, hangs, and incidents.
- Reviewing code for real bugs and security vulnerabilities.
- Auditing auth/authz boundaries, secrets, injections, races, unsafe code.
- Dependency vulnerability assessment with reachability checks.
- Production-impacting changes that need careful review.

## Prerequisites

- Git (for diff/status checks).
- Language-specific tools are optional but recommended:
  - Python: `pytest`, `bandit`, `pip-audit` (if available)
  - Rust: `cargo`, `clippy`, `cargo-audit`, `miri` (if available)
  - Go: `go`, `govulncheck`, `gosec` (if available)
  - Shell: `shellcheck`, `shfmt`, `bats` (if available)
- Do not install missing tools without approval. Report them under
  "Checks not run" instead.

## How to Run

1. Identify the mode: Debug, Audit, or Incident (see table below).
2. Follow the workflow for that mode.
3. Use `search_files` for pattern discovery, `read_file` for inspection.
4. Produce output in the format specified in `references/report-format.md`.

## Quick Reference

| Task | Mode | Lead with |
| --- | --- | --- |
| Bug, crash, flaky test, regression, hang | Debug | reproduction state and evidence |
| PR/repo/security review, "find bugs" | Audit | verdict and findings |
| Secret leak, exploitation, prod incident | Incident | evidence preservation |

## Procedure

### Core Rule

Prove root cause before fixing; reachability or exploitability before
reporting security. Do not accept requested severity without evidence.

### Workflow

1. Capture exact symptom or review scope.
2. Inspect diff, entry points, trust boundaries, tests, configs, assets.
3. Gather logs, stack traces, failing tests, tool output, data/control flow.
4. Trace source -> validation -> transformation -> sink, or trigger -> state -> failure.
5. Test one hypothesis at a time with the smallest safe check.
6. Fix after proof; label candidate fixes as provisional.
7. Verify with targeted tests plus a small broad check.
8. Report checks run, checks not run, and residual risk.

### Security Gate

Do not confirm a security issue from pattern-matching alone. Show at least one:

- controlled input reaches a dangerous sink;
- auth/authz/tenant boundary is missing;
- secret is exposed, logged, committed, or passed unsafely;
- vulnerable dependency is present and plausibly reachable;
- unsafe/concurrent/filesystem/network/crypto invariant is violated.

If unclear, write `Hypothesis` and give the verification path. Never print
secrets. `unsafe` without code/invariant proof is not confirmed critical.

### First Checks

Prefer existing project scripts. Do not install tools without approval.

Use `search_files` with a regex pattern to find risky symbols across the repo:

```text
TODO|FIXME|XXX|HACK|password|secret|token|api[_-]?key|private[_-]?key|BEGIN RSA|BEGIN OPENSSH|DATABASE_URL|JWT|SESSION|DEBUG|shell=True|unsafe|exec\.|Command\(|rm -rf|0\.0\.0\.0|privileged|hostNetwork|docker\.sock
```

Use `terminal` for git diff and status:

```bash
git status --short
git diff --stat
git diff --check
git diff --name-only
```

### Stop And Reassess

Stop when three hypotheses fail, reproduction is missing, evidence contradicts
the theory, severity was requested but not proved, or the fix touches auth,
secrets, migrations, deletion, prod infra, or data loss.

### Output

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

Every finding needs file/line, proof, scenario, impact, fix, verification.
If no blocker is found, say so and list residual risk.

See `references/report-format.md` for full output templates and examples.

## References

- Method, OWASP/CWE/NIST: `references/security-method.md`
- Python: `references/python.md`
- Rust: `references/rust.md`
- Go: `references/go.md`
- Shell: `references/shell.md`
- Output format: `references/report-format.md`
- Skill pressure tests: `references/pressure-scenarios.md`

## Pitfalls

| Trap | Counter |
| --- | --- |
| "The fix is obvious." | Still prove the cause. |
| "The scanner says vulnerable." | Check version, usage, reachability, impact. |
| "Internal only." | Internal paths still need guardrails. |
| "Mark all unsafe critical." | Refuse until code proves invariant violation. |
| "Quoted shell vars are enough." | Also check empties, roots, globs, `--`, traps. |

## Verification

To verify this skill is working correctly:

1. Run a pressure scenario from `references/pressure-scenarios.md` (e.g.
   RED-02 false SQL injection) and confirm the agent does not overstate
   severity without tracing upstream validation.
2. Run RED-05 Rust unsafe FFI and confirm the agent reports violated
   invariants, not just the presence of `unsafe`.
3. Confirm output follows the format in `references/report-format.md`
   with file/line, proof, scenario, impact, fix, and verification.