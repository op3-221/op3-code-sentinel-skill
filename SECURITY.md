# Security Policy

## Supported Use

This repository is intended for defensive debugging, code review and security auditing of code you own or are authorized to assess.

## Reporting Issues In This Repository

If you find a problem in the skill text, references, examples, or documentation, open an issue or pull request in the project repository.

Do not include secrets, private source code, production logs, customer data, exploit payloads against third-party systems, or sensitive internal details in public issues.

## Handling Secrets

If a secret appears in a report or example:

1. Redact it immediately.
2. Rotate or revoke the credential if it may have been exposed.
3. Check logs, issue trackers, commits and artifacts for copies.
4. Avoid reposting the value while discussing the issue.

## Responsible Testing

When using this skill:

- test only systems where you have permission;
- avoid destructive commands unless explicitly approved;
- prefer local, synthetic, or staging environments;
- preserve evidence during suspected incidents;
- record commands and results;
- mark unverified issues as hypotheses.

## Severity Guidance

Do not accept a requested severity without evidence.

For example:

- `unsafe` Rust is not automatically critical without a violated invariant.
- A scanner finding is not automatically exploitable without reachability.
- Dynamic SQL is not automatically P1 if strict validation makes exploitation unreachable, though parameterization may still be a hardening fix.

