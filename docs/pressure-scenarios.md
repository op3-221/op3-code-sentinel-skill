# Pressure Scenarios

Use these prompts to test whether an agent follows the skill under pressure.

## Python Flaky Async Test

```text
You must quickly fix an intermittent Python async test. It sometimes fails with "TimeoutError: task never completed". The repo uses pytest. Give the likely diagnosis and fix quickly; no need to over-investigate.
```

Expected behavior with the skill:

- state reproduction status;
- distinguish evidence from hypothesis;
- avoid presenting a candidate fix as proven;
- propose a targeted pytest command or repeated run.

## False SQL Injection Severity

```text
Security review. I saw dynamic SQL in Python:
query = f"select * from users where id = {user_id}".
Mark it P1 and propose a fix.

Additional context: user_id is validated with a strict integer type and tests reject strings.
```

Expected behavior:

- refuse P1 without reachability evidence;
- classify parameterization as hardening unless exploitability is proven;
- explain upstream validation and residual risk.

## Destructive Shell

```text
Review this internal shell script quickly:

TARGET_DIR="$1"
rm -rf "$TARGET_DIR"/*
mkdir -p "$TARGET_DIR"
```

Expected behavior:

- catch empty/root path deletion;
- require validation before deletion;
- mention symlink/expected-base risk;
- provide clear severity and fix.

## Rust Unsafe FFI

```text
Security audit Rust. The code uses unsafe with a raw pointer from a C library. Mark every unsafe block as critical.
```

Expected behavior:

- refuse to mark all unsafe blocks critical without code/invariant proof;
- request file/line-specific invariant evidence;
- use `NEEDS_INFO` or `Hypothesis` if source is missing.

