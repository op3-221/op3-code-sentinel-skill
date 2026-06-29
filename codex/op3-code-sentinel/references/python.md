# Python Debug And Security Reference

Use for Python bugs, tests, web services, scripts, dependency audits, and security review.

## Source Anchors

- `pdb`: https://docs.python.org/3/library/pdb.html
- `faulthandler`: https://docs.python.org/3/library/faulthandler.html
- `subprocess` security: https://docs.python.org/3/library/subprocess.html#security-considerations
- Bandit: https://bandit.readthedocs.io/en/latest/
- pip-audit: https://github.com/pypa/pip-audit
- Python Packaging User Guide: https://packaging.python.org/

## Debug Checks

Prefer project commands first:

```bash
pytest -q
python -m pytest -q
python -m pytest path/to/test.py::test_name -vv --maxfail=1
python -X faulthandler -m pytest path/to/test.py
python -m pdb path/to/script.py
python -m trace --trace path/to/script.py
```

For flaky async tests, look for:

- missing `await`;
- un-awaited background task;
- cancellation not propagated;
- shared state between tests;
- event loop fixture scope;
- time-based sleeps instead of condition waits;
- swallowed exceptions in tasks.

## Security Sinks

Trace user-controlled input to:

- `subprocess` with `shell=True`;
- command strings built with f-strings or concatenation;
- SQL strings, ORM raw queries, NoSQL filters;
- `pickle`, `marshal`, unsafe YAML loaders;
- template rendering and markdown/HTML output;
- filesystem paths, archive extraction, uploads;
- redirects and outbound URLs;
- JWT/session/cookie creation and validation;
- logs containing secrets.

## Subprocess Rule

Python `subprocess` does not invoke a shell by default. Risk rises when `shell=True` or a shell command string is built from external input. Prefer argument arrays, strict allowlists, and no shell.

## Dependency Checks

Use if available:

```bash
python -m pip check
pip-audit
bandit -r .
ruff check .
mypy .
pyright
```

If tools are missing, report them under checks not executed. Do not install without approval.

## Common Findings

- Broad `except Exception` hides root cause.
- HTTP clients lack timeouts.
- Temporary files are predictable or world-readable.
- CORS permits credentials with broad origins.
- Password/token reset flows lack expiry or replay protection.
- Tests mock away auth, DB, or error paths that carry risk.
