---
name: op3-code-sentinel
description: Debug bugs and audit code with proof before fixes.
version: 0.2.0
author: OP3 Maintainers
metadata:
  hermes:
    tags: [debugging, code-review, security, python, rust, go, shell]
    category: code-review
---

# OP3 Code Sentinel Skill

Debug real bugs, crashes, regressions, flaky tests, incidents, and security
issues in Python, Rust, Go, Shell, and infrastructure code. This skill is the
Hermes-native package for the Skills Hub; it uses Hermes tool names and install
conventions.

## When to Use

- Debugging crashes, regressions, flaky tests, hangs, and incidents.
- Reviewing code for real bugs and security vulnerabilities.
- Auditing auth/authz boundaries, secrets, injections, races, unsafe code.
- Assessing dependency vulnerabilities with reachability checks.
- Reviewing production-impacting changes that need careful evidence.

## Prerequisites

- Install through Hermes Skills Hub when possible:
  `hermes skills install op3-221/op3-code-sentinel-skill/skills/op3-code-sentinel`
- If direct GitHub install fails on an older Hermes version, add the tap first:
  `hermes skills tap add op3-221/op3-code-sentinel-skill`.
- Use Hermes-native tools in prose: `search_files`, `read_file`, and
  `terminal`.
- Language-specific CLIs are optional. Do not install missing tools without
  approval; list missing checks under "Checks not run".

## How to Run

1. Identify the mode: Debug, Audit, or Incident.
2. Use `search_files` for pattern discovery and `read_file` for inspection.
3. Use `terminal` for existing project checks such as git diffs or tests.
4. Load the relevant reference file only when the task calls for it.
5. Report using the format in `references/report-format.md`.

## Quick Reference

| Task | Mode | Lead with |
| --- | --- | --- |
| Bug, crash, flaky test, regression, hang | Debug | reproduction state and evidence |
| PR/repo/security review, "find bugs" | Audit | verdict and findings |
| Secret leak, exploitation, prod incident | Incident | evidence preservation |

## Procedure

### Core Rule

Prove root cause before fixing. Prove reachability or exploitability before
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

Use `search_files` with this risky-symbol pattern:

```text
TODO|FIXME|XXX|HACK|password|secret|token|api[_-]?key|private[_-]?key|BEGIN RSA|BEGIN OPENSSH|DATABASE_URL|JWT|SESSION|DEBUG|shell=True|unsafe|exec\.|Command\(|0\.0\.0\.0|privileged|hostNetwork|docker\.sock
```

Use `terminal` for these git checks when reviewing a repository:

```bash
git status --short
git diff --stat
git diff --check
git diff --name-only
```

### Stop And Reassess

Stop when three hypotheses fail, reproduction is missing, evidence contradicts
the theory, severity was requested but not proved, or the fix touches auth,
secrets, migrations, deletion, production infrastructure, or data loss.

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

Every finding needs file/line, proof, scenario, impact, fix, and verification.
If no blocker is found, say so and list residual risk.

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

1. Run RED-02 from `references/pressure-scenarios.md` and confirm the agent
   refuses to overstate SQL injection severity without tracing validation.
2. Run RED-05 and confirm unsafe Rust findings cite violated invariants, not
   the mere presence of `unsafe`.
3. Confirm review output follows `references/report-format.md`.
