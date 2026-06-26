# Go Debug And Security Reference

Use for Go bugs, races, goroutine leaks, HTTP services, CLI tools, dependency audits, and security review.

## Source Anchors

- Go diagnostics: https://go.dev/doc/diagnostics
- Go race detector: https://go.dev/doc/articles/race_detector
- Go security best practices: https://go.dev/doc/security/best-practices
- govulncheck tutorial: https://go.dev/doc/tutorial/govulncheck
- govulncheck command: https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck
- gosec: https://github.com/securego/gosec

## Debug Checks

Prefer project commands first:

```bash
go test ./...
go test -run TestName ./path
go test -race ./...
go test -cover ./...
go vet ./...
```

Use for deeper diagnosis:

```bash
go test -cpuprofile cpu.out -memprofile mem.out ./...
go tool pprof cpu.out
go test -trace trace.out ./...
```

Race detector output is evidence only for exercised paths. Add a targeted test or workload when the path is not covered.

## Security Checks

Use if available:

```bash
govulncheck ./...
gosec ./...
```

`govulncheck` helps prioritize vulnerabilities by code usage. Still inspect impact and upgrade path.

## Review Targets

- ignored errors;
- missing `context.Context` propagation;
- HTTP client/server without timeouts;
- response body not closed;
- goroutine leaks and blocked channels;
- concurrent map writes or shared mutable state;
- `exec.Command` fed by unvalidated input;
- path traversal with joined paths;
- `math/rand` used for secrets;
- TLS verification disabled;
- SSRF via user-controlled URLs;
- authz missing for object/tenant access;
- transaction rollback/commit errors ignored.

## Concurrency Triage

For race/deadlock/leak:

1. Identify goroutine ownership and shared state.
2. Reproduce with `go test -race` or targeted workload.
3. Choose synchronization by data ownership: mutex/RWMutex, channel ownership, atomic, or `sync.Map`.
4. Verify with race detector and relevant unit/integration test.
