# Rust Debug And Security Reference

Use for Rust bugs, unsafe code, FFI, concurrency, panics, dependency audits, and memory-safety review.

## Source Anchors

- Clippy: https://doc.rust-lang.org/clippy/
- Miri: https://github.com/rust-lang/miri
- Rust sanitizers: https://doc.rust-lang.org/beta/unstable-book/compiler-flags/sanitizer.html
- RustSec: https://rustsec.org/
- cargo-audit: https://github.com/rustsec/rustsec/tree/main/cargo-audit
- Rustonomicon: https://doc.rust-lang.org/nomicon/

## Debug Checks

Use project commands first:

```bash
cargo test
cargo test --all-features
cargo test test_name
cargo clippy --all-targets --all-features -- -D warnings
cargo fmt --check
```

Use when available and appropriate:

```bash
cargo audit
cargo miri test
RUSTFLAGS="-Z sanitizer=address" cargo +nightly test -Zbuild-std
```

Miri and sanitizers are powerful but not universal. Note platform/nightly limits and do not claim absence of all bugs.

## Unsafe Review

`unsafe` is not automatically a vulnerability and is not automatically critical. Review the invariant:

- pointer non-null and aligned;
- length/capacity valid;
- aliasing rules respected;
- ownership/lifetime clear;
- C ABI and layout correct;
- no double free or use-after-free;
- thread-safety/Send/Sync justified;
- panic across FFI boundary avoided;
- caller obligations documented.

Report the violated invariant, not just the presence of `unsafe`. If source code is missing or the invariant cannot be checked, use `NEEDS_INFO` or `Hypothesis`; do not mark every unsafe block critical from the word `unsafe` alone.

## Security Sinks

Trace external input to:

- FFI pointers and lengths;
- `std::process::Command`;
- filesystem paths and archive extraction;
- deserialization;
- crypto/random/token generation;
- network clients with TLS changes;
- auth/session parsing;
- SQL/raw query strings.

## Common Findings

- `unwrap` or `expect` reachable from external input in services/CLI.
- Panics in library APIs instead of returning `Result`.
- Errors converted to strings too early, losing context.
- Async locks held across `.await`.
- Global mutable state without synchronization.
- Dependency advisories ignored without reachability assessment.
