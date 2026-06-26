# Disclaimer

This project is provided as-is, without warranty of any kind.

The skill, examples and documentation are intended to help AI coding agents reason more carefully about debugging and security review. They do not guarantee that an agent will find every bug, every vulnerability, every exposed secret, or every production risk.

## No Warranty

The software and documentation are provided "as is", without warranty of any kind, express or implied, including but not limited to warranties of merchantability, fitness for a particular purpose and noninfringement.

The authors and contributors are not liable for claims, damages or other liability arising from use of this project.

The MIT license in `LICENSE` contains the binding license text. This file explains the intended practical meaning in plain language.

## No Support Obligation

Publishing this project does not create an obligation to provide support, maintenance, fixes, updates, security audits, incident response, consulting, or training.

Users are responsible for deciding whether the project is appropriate for their environment and for validating all outputs before use.

## Not Professional Advice

This project is not legal, compliance, financial, operational, or professional security advice.

For high-impact systems, regulated environments, incident response, breach handling, disclosure decisions, legal obligations, or production-critical remediation, involve qualified humans and follow your organization's procedures.

## Responsible Use

Use this skill only on code and systems you own or are authorized to test.

Do not use it to exploit third-party systems, steal data, bypass authorization, deploy malware, or hide compromise.

When reviewing security issues:

- do not print secrets;
- redact credentials in reports;
- preserve evidence during incidents;
- avoid destructive actions unless explicitly authorized;
- verify findings before escalating severity;
- distinguish confirmed vulnerabilities from hypotheses;
- document checks that were not run.

## AI Limitations

AI agents can miss vulnerabilities, invent problems, misunderstand code, misclassify severity, or propose unsafe fixes.

Treat all output as review assistance, not final truth. Run tests, inspect patches, and require human approval for sensitive changes.

## Safe Publication Checklist

Before publishing your own fork:

- remove private paths, usernames, tokens and internal hostnames;
- remove real secrets and production logs;
- avoid publishing proprietary code or incident details;
- keep examples synthetic or clearly authorized;
- include a license and disclaimer;
- describe limitations honestly.
