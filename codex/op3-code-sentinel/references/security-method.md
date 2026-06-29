# Security Method Reference

Use this reference for security audits, code reviews, incident triage, and severity calls.

## Source Anchors

- OWASP Code Review Guide: https://owasp.org/www-project-code-review-guide/
- OWASP Web Security Testing Guide: https://owasp.org/www-project-web-security-testing-guide/
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- MITRE CWE Top 25: https://cwe.mitre.org/top25/
- NIST SSDF SP 800-218: https://csrc.nist.gov/pubs/sp/800/218/final

## Finding Proof Ladder

Prefer findings with higher proof:

1. Confirmed exploit or failing security test.
2. Reachable data flow from controlled input to dangerous sink.
3. Missing auth/authz check on a reachable operation.
4. Unsafe config exposed in deployment.
5. Vulnerable dependency with used symbol/reachable path.
6. Suspicious pattern with missing context.

Levels 1-5 can usually become findings. Level 6 is a hypothesis until verified.

## Threat Modeling Quick Pass

Identify:

- actor: anonymous user, authenticated user, tenant user, admin, integration, cron, local user;
- asset: secret, personal data, money, permissions, filesystem, network, model/tool access;
- boundary: HTTP, CLI args, file upload, webhook, queue, database, template, subprocess, container;
- sink: SQL, shell, filesystem, HTML, redirect, deserialization, crypto, token/session, network request.

Trace: source -> validation -> transformation -> sink -> impact.

## OWASP/CWE Mapping

Map important findings when useful:

- Injection: SQL, shell, template, LDAP, NoSQL, command.
- Broken access control: IDOR, missing tenant check, admin endpoint exposed.
- Auth/session: weak token validation, missing expiry, bad cookie flags.
- Cryptographic failures: custom crypto, weak randomness, TLS disabled, secrets logged.
- Security misconfiguration: debug mode, public admin port, permissive CORS.
- Vulnerable/outdated components: dependency issue with reachability.
- SSRF/path traversal/deserialization/open redirect: prove source and sink.

Do not force a mapping when it adds noise.

## Severity

- P0 Critical: RCE, secret exposed, auth bypass major, data loss, destructive prod break.
- P1 High: exploitable flaw likely, major regression, exposed sensitive service, serious race/data corruption.
- P2 Medium: plausible bug/risk with conditions, missing validation, robustness issue.
- P3 Low: maintainability, minor edge case, docs, style.
- Info: useful observation without requested change.

Do not request changes for P3 only.

## Anti False Positive Gate

Before reporting:

1. Is the path reachable?
2. Who controls the input?
3. Is there upstream validation?
4. Does the framework already neutralize the sink?
5. Is there a test or runtime guard?
6. Is the impact concrete?
7. Is the proposed fix compatible with existing behavior?

If any answer is unknown, either gather evidence or mark the issue as `Hypothesis`.

## Incident Rules

For possible exploitation or secret leak:

- preserve evidence;
- do not print secrets;
- redact logs and values;
- separate containment, root cause, and permanent remediation;
- recommend rotation/revocation for exposed credentials;
- avoid destructive cleanup unless explicitly authorized.
