# Publishing Guide

This guide prepares OP3 Code Sentinel Skill for a public Git repository.

Recommended public name:

```text
OP3 Code Sentinel Skill
```

Recommended GitHub repository:

```text
op3-code-sentinel-skill
```

Recommended GitHub description:

```text
Portable AI-agent skill for code review, debugging, and security audits. Includes Hermes, Codex, OpenCode, and generic packages.
```

## 1. Review Before Publishing

Check that the repository does not contain:

- private paths;
- usernames;
- hostnames;
- IP addresses;
- API keys;
- production logs;
- real customer data;
- internal prompts or private agent profiles.

Suggested checks:

```bash
rg -n "/Users/|HOME|API[_-]?KEY|SECRET|TOKEN|PASSWORD|BEGIN RSA|BEGIN OPENSSH|PRIVATE KEY|localhost|127\\.0\\.0\\.1|[0-9]+\\.[0-9]+\\.[0-9]+\\.[0-9]+" .
```

False positives are normal. Review each match.

## 2. Initialize Git

From this directory:

```bash
git init
git add .
git commit -m "Initial public release"
```

## 3. Create A GitHub Repository

Create an empty repository on GitHub:

```text
op3-code-sentinel-skill
```

Then push:

```bash
git branch -M main
git remote add origin git@github.com:YOUR_ACCOUNT/op3-code-sentinel-skill.git
git push -u origin main
```

## 4. Suggested Repository Settings

- Add topics: `ai-agent`, `skill`, `skills`, `code-review`, `debugging`, `security-audit`, `static-analysis`, `hermes`, `codex`, `python`, `rust`, `go`, `shell`, `op3`.
- Disable Issues if you do not want support requests.
- Enable issue templates if you expect community feedback.
- Require pull request review if multiple maintainers are involved.
- Keep examples synthetic and authorized.

## 5. Release Checklist

- `README.md` explains installation and usage.
- `HERMES.md` explains that the Hermes package intentionally lives under
  `skills/op3-code-sentinel/`.
- `skills/op3-code-sentinel/` installs through `hermes skills install`.
- `codex/op3-code-sentinel/` is the Codex package.
- `.opencode/skills/op3-code-sentinel/` is the OpenCode package.
- `portable/op3-code-sentinel/` is the runtime-neutral package.
- `skills.sh.json` groups the Hermes tap skill for catalog display.
- `LICENSE` is present.
- `DISCLAIMER.md` is present.
- `SECURITY.md` is present.
- `examples/bugsy-soul/SOUL.md` is generic.
- The skill validates:

```bash
python3 /path/to/quick_validate.py skills/op3-code-sentinel
python3 /path/to/quick_validate.py codex/op3-code-sentinel
python3 /path/to/quick_validate.py .opencode/skills/op3-code-sentinel
python3 /path/to/quick_validate.py portable/op3-code-sentinel
```
