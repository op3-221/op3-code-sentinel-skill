# OP3 Code Sentinel Skill

**OP3 Code Sentinel Skill** is a community skill for AI coding agents that
debug real bugs and audit code security with evidence.

Core rule:

```text
Prove root cause before fixing.
Prove reachability or exploitability before reporting security.
Do not accept requested severity without evidence.
```

## What It Helps With

- Debugging crashes, regressions, flaky tests, hangs, and incidents.
- Reviewing code for real bugs and security vulnerabilities.
- Avoiding false positives in security reports.
- Forcing source -> validation -> sink reasoning.
- Producing actionable findings with proof, impact, fix, and verification.
- Handling Python, Rust, Go, Shell, infrastructure scripts, auth/authz,
  secrets, injections, races, unsafe code, and dependency vulnerabilities.

## Runtime Packages

This repository ships four installable variants so each agent gets native
instructions and install paths.

```text
skills/op3-code-sentinel/              Hermes Skills Hub package
codex/op3-code-sentinel/               Codex package
.opencode/skills/op3-code-sentinel/    OpenCode package
portable/op3-code-sentinel/            Runtime-neutral package
```

The methodology references are copied into each package so every variant can
be installed on its own.

## Install In Hermes

Hermes must be installed through the Skills Hub command so the skill appears in
Hermes skill lists, can be audited, and can be updated later. Do not install the
Hermes variant by manually copying files into `~/.hermes/skills`.

Preview:

```bash
hermes skills inspect op3-221/op3-code-sentinel-skill/skills/op3-code-sentinel
```

Install:

```bash
hermes skills install op3-221/op3-code-sentinel-skill/skills/op3-code-sentinel
```

Verify:

```bash
hermes skills list --source hub
```

If direct GitHub install is unavailable in your Hermes version, add the repo as
a tap and install the identifier returned by search:

```bash
hermes skills tap add op3-221/op3-code-sentinel-skill
hermes skills search op3 --source github
hermes skills install <identifier-from-search>
```

Inside a Hermes chat session, the same flow is available through slash
commands:

```text
/skills inspect op3-221/op3-code-sentinel-skill/skills/op3-code-sentinel
/skills install op3-221/op3-code-sentinel-skill/skills/op3-code-sentinel
/skills list
```

## Install In Codex

The Codex package is kept outside `.agents/skills/` in this repository to avoid
OpenCode loading the Codex variant through its compatibility scanner. Install it
into Codex's normal user skill directory.

For a personal user-wide install:

```bash
mkdir -p ~/.agents/skills
rsync -a codex/op3-code-sentinel/ ~/.agents/skills/op3-code-sentinel/
```

For repo-scoped Codex use in another project, copy the same directory to that
project's `.agents/skills/op3-code-sentinel/`. Start a new Codex thread if the
skill does not appear immediately. The Codex package includes
`agents/openai.yaml` for Codex UI metadata.

Example prompt:

```text
Use $op3-code-sentinel to review this code for real bugs and security risks.
Prove reachability before reporting findings.
```

## Install In OpenCode

OpenCode can use the project-scoped skill directly when this repository is
open, because the OpenCode package lives under `.opencode/skills/`.

For a global OpenCode install:

```bash
mkdir -p ~/.config/opencode/skills
rsync -a .opencode/skills/op3-code-sentinel/ ~/.config/opencode/skills/op3-code-sentinel/
```

Use OpenCode's normal skill invocation and ask for `op3-code-sentinel` when
debugging or reviewing code.

## Install In Other Agents

Use the runtime-neutral package when your agent supports the open
`SKILL.md`-style layout but has different tool names:

```bash
portable/op3-code-sentinel/
```

Adapt mentions of file search, file reading, shell execution, and patching to
the target runtime.

## Repository Layout

```text
skills/op3-code-sentinel/
  SKILL.md
  references/

codex/op3-code-sentinel/
  SKILL.md
  agents/openai.yaml
  references/

.opencode/skills/op3-code-sentinel/
  SKILL.md
  references/

portable/op3-code-sentinel/
  SKILL.md
  references/

docs/
examples/
skills.sh.json
DISCLAIMER.md
SECURITY.md
SUPPORT.md
LICENSE
PUBLISHING.md
```

## Validation

The skill was tested with pressure scenarios:

- Python flaky async test.
- False SQL injection severity request.
- Destructive Shell snippet.
- Rust unsafe FFI severity trap.

One Rust scenario initially failed because the agent accepted "mark all unsafe
critical" too readily. The skill was then hardened and retested successfully.

## Safety

Read `DISCLAIMER.md` and `SECURITY.md` before using this in production or
security-sensitive work.

This project helps agents reason better. It does not replace human review,
professional security testing, legal advice, compliance work, or production
incident response.

## Support

This project is shared as a community resource with no support obligation. See
`SUPPORT.md`.
