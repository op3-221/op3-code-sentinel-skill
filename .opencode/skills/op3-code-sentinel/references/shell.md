# Shell Debug And Security Reference

Use for Bash/sh scripts, installers, deployment scripts, CI scripts, Docker/systemd glue, and destructive command review.

## Source Anchors

- ShellCheck: https://www.shellcheck.net/
- ShellCheck wiki: https://www.shellcheck.net/wiki/Home
- Google Shell Style Guide: https://google.github.io/styleguide/shellguide.html
- GNU Bash manual: https://www.gnu.org/software/bash/manual/bash.html
- BashFAQ 105 for `set -e`: https://mywiki.wooledge.org/BashFAQ/105

## Checks

Use if available:

```bash
bash -n script.sh
shellcheck script.sh
shfmt -d script.sh
bats test/
```

Do not rely on `set -e` alone. It has exceptions and does not replace explicit checks around dangerous operations.

## High-Risk Patterns

- unquoted variables;
- word splitting or globbing;
- `rm -rf`, `mv`, `cp`, `chown`, `chmod` using variables;
- empty target variables;
- root path or parent directory target;
- `curl | bash`;
- temporary files without `mktemp`;
- missing `trap` cleanup;
- secrets in argv, logs, history, or env dumps;
- broad `sudo`;
- scripts that are not idempotent;
- interactive prompts in automation;
- firewall, SSH, Docker, or systemd changes without explicit scope.

## Destructive Command Gate

Before approving destructive commands, require:

```bash
: "${TARGET_DIR:?TARGET_DIR is required}"
case "$TARGET_DIR" in
  "/"|"/root"|"/home"|"/var"|"/usr"|"/etc"|"") exit 1 ;;
esac
```

Also check:

- path is under expected base;
- command uses `--` before paths where supported;
- glob behavior is intentional;
- dry run or logging exists for broad operations;
- cleanup happens after successful validation, not before.

## Installer Script Review

Verify:

- official sources only;
- checksums/signatures when relevant;
- retries do not switch to unofficial mirrors;
- no secrets in command line;
- non-interactive behavior when automated;
- idempotence;
- clear stop on APT/Docker/GitHub/provider outage;
- services not started before required manual onboarding.
