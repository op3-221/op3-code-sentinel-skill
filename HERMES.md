# Hermes Install

The Hermes package for this repository is:

```text
skills/op3-code-sentinel/
```

This directory name is intentional. Hermes Skills Hub taps discover skills from
the standard layout:

```text
skills/<skill-name>/SKILL.md
```

Do not rename the Hermes package to `hermes/` unless you also change the
published install commands and tap layout.

## Recommended Install

Preview the skill:

```bash
hermes skills inspect op3-221/op3-code-sentinel-skill/skills/op3-code-sentinel
```

Install the skill:

```bash
hermes skills install op3-221/op3-code-sentinel-skill/skills/op3-code-sentinel
```

Verify that Hermes can list it:

```bash
hermes skills list --source hub
```

## Older Hermes Fallback

If direct GitHub install is unavailable in your Hermes version, add the
repository as a tap and install the identifier returned by search:

```bash
hermes skills tap add op3-221/op3-code-sentinel-skill
hermes skills search op3 --source github
hermes skills install <identifier-from-search>
```

Manual copying into `~/.hermes/skills` is not recommended for this repository,
because that bypasses the Skills Hub install path and may prevent the skill from
appearing in hub-based skill lists.
