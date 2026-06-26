# OP3 Code Sentinel Skill

**OP3 Code Sentinel Skill** is a portable community skill for AI coding agents that need to debug real bugs and audit code security with evidence.

It is designed for Python, Rust, Go, Shell, infrastructure scripts, authentication, authorization, secrets, injections, races, unsafe code, dependency vulnerabilities and production-impacting changes.

It includes ready-to-use install notes for Hermes and Codex. Other agent runtimes can adapt the `SKILL.md` workflow and references.

Core rule:

```text
Prove root cause before fixing.
Prove reachability or exploitability before reporting security.
Do not accept requested severity without evidence.
```

## Why This Name

`debug` is too narrow. This skill does more than debug: it watches trust boundaries, follows data flow, checks exploitability, resists false positives and forces reproducible evidence.

`OP3 Code Sentinel` means:

- code-aware;
- defensive;
- evidence-first;
- useful for debug and security review.

Technical skill id:

```text
op3-code-sentinel
```

Suggested public repository name:

```text
op3-code-sentinel-skill
```

Suggested repository description:

```text
Portable AI-agent skill for code review, debugging, and security audits. Includes Hermes and Codex install examples.
```

## What It Helps With

- Debugging crashes, regressions, flaky tests, hangs and incidents.
- Reviewing code for real bugs and security vulnerabilities.
- Avoiding false positives in security reports.
- Forcing source -> validation -> sink reasoning.
- Producing actionable findings with proof, impact, fix and verification.
- Handling Python, Rust, Go and Shell with language-specific references.

## Repository Layout

```text
skills/op3-code-sentinel/
  SKILL.md
  agents/openai.yaml
  references/
    security-method.md
    python.md
    rust.md
    go.md
    shell.md
    report-format.md
    pressure-scenarios.md

docs/
  research.md
  pressure-scenarios.md
  validation-results.md

examples/
  bugsy-soul/SOUL.md

DISCLAIMER.md
SECURITY.md
SUPPORT.md
LICENSE
PUBLISHING.md
```

## Compatibility

This package is intentionally not locked to one agent platform.

- Hermes: copy the skill into `~/.hermes/skills/`.
- Codex: copy the skill into `~/.codex/skills/`.
- Other AI-agent runtimes: adapt `skills/op3-code-sentinel/SKILL.md` and the reference files to the runtime's skill/profile format.

The example soul in `examples/bugsy-soul/SOUL.md` is Hermes-oriented, but the core review methodology is platform-neutral.

## Install In Hermes

Copy the skill folder into the Hermes skill directory:

```bash
mkdir -p ~/.hermes/skills
rsync -a skills/op3-code-sentinel/ ~/.hermes/skills/op3-code-sentinel/
```

Restart the Hermes session or gateway that should load the skill.

## Install In Codex

Copy the skill folder into the Codex skill directory:

```bash
mkdir -p ~/.codex/skills
rsync -a skills/op3-code-sentinel/ ~/.codex/skills/op3-code-sentinel/
```

Start a new Codex session so the skill metadata is discovered.

## Example Prompt

```text
Use the op3-code-sentinel skill.
Review this code for real bugs and security risks.
Prove reachability before reporting findings.
```

## Example Agent Soul

See:

```text
examples/bugsy-soul/SOUL.md
```

This is an example only. Adapt the name, orchestration and operational boundaries to your own environment.

## Validation

The skill was tested with pressure scenarios:

- Python flaky async test.
- False SQL injection severity request.
- Destructive Shell snippet.
- Rust unsafe FFI severity trap.

One Rust scenario initially failed because the agent accepted "mark all unsafe critical" too readily. The skill was then hardened and retested successfully.

## Safety

Read `DISCLAIMER.md` and `SECURITY.md` before using this in production or security-sensitive work.

This project helps agents reason better. It does not replace human review, professional security testing, legal advice, compliance work, or production incident response.

## Support

This project is shared as a community resource with no support obligation. See `SUPPORT.md`.
