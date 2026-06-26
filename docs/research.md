# Research Summary

This skill was designed around one core failure mode: agents often guess fixes or report security findings from suspicious patterns without proving root cause, reachability or exploitability.

## Primary Sources

- OWASP Code Review Guide: https://owasp.org/www-project-code-review-guide/
- OWASP Web Security Testing Guide: https://owasp.org/www-project-web-security-testing-guide/
- OWASP Top 10: https://owasp.org/www-project-top-ten/
- MITRE CWE Top 25: https://cwe.mitre.org/top25/
- NIST SSDF SP 800-218: https://csrc.nist.gov/pubs/sp/800/218/final
- Python `pdb`: https://docs.python.org/3/library/pdb.html
- Python `faulthandler`: https://docs.python.org/3/library/faulthandler.html
- Python `subprocess` security: https://docs.python.org/3/library/subprocess.html#security-considerations
- Bandit: https://bandit.readthedocs.io/en/latest/
- pip-audit: https://github.com/pypa/pip-audit
- Rust Clippy: https://doc.rust-lang.org/clippy/
- Rust Miri: https://github.com/rust-lang/miri
- Rust sanitizers: https://doc.rust-lang.org/beta/unstable-book/compiler-flags/sanitizer.html
- RustSec: https://rustsec.org/
- Go diagnostics: https://go.dev/doc/diagnostics
- Go race detector: https://go.dev/doc/articles/race_detector
- Go security best practices: https://go.dev/doc/security/best-practices
- govulncheck: https://go.dev/doc/tutorial/govulncheck
- ShellCheck: https://www.shellcheck.net/
- Google Shell Style Guide: https://google.github.io/styleguide/shellguide.html
- GNU Bash manual: https://www.gnu.org/software/bash/manual/bash.html

## Design Recommendations

- Keep `SKILL.md` compact.
- Put language-specific details in `references/`.
- Require evidence before severity.
- Require source -> validation -> sink tracing for security findings.
- Require root-cause proof before fixes.
- Include explicit rationalization traps.
- Test the skill with adversarial pressure scenarios.

