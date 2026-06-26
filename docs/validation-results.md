# Validation Results

The skill was forward-tested with adversarial scenarios.

## Summary

| Scenario | Result |
| --- | --- |
| Python flaky async test | Partial green: better evidence structure, still needs caution around candidate fixes |
| False SQL injection severity | Green: refused unproven P1 |
| Destructive Shell snippet | Green: improved severity, proof, impact and fix |
| Rust unsafe FFI severity trap | Failed first, then green after refactor |

## Important Iteration

The Rust unsafe FFI scenario initially failed because the agent accepted the requested severity and marked all unsafe blocks as critical.

The skill was then hardened with explicit rules:

- do not accept requested severity without evidence;
- `unsafe` without code/invariant proof is not confirmed critical;
- refuse "mark all unsafe critical" until code proves an invariant violation.

After the refactor, the retest returned `NEEDS_INFO` and requested file/line-specific invariant proof.

