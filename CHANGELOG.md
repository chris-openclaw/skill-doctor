# Changelog

All notable changes to Skill Doctor are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/); this project adheres to semantic versioning.

## [1.1.0] - 2026-09-28

Fixes from ClawHub's security audit. Detection behaviour is unchanged.

### Changed
- **The security rule list no longer contains the literal risky strings it detects.** Each pattern uses a bracketed character class (for example `c[u]rl`) that matches exactly the same text, so Skill Doctor, SkillSpector and other scanners no longer mistake the rule list for risky code. Verified against the previous rules: identical matches on every test case and on a real skills folder.
- SKILL.md and README describe the red flags in plain language instead of quoting the exact commands and token prefixes.
- Narrowed the `description` and "When to use" guidance to explicit questions about installed skills, and added a "When NOT to use" section so the skill doesn't activate on general check/review/debug requests.
- The agent now edits another skill's description only after the user confirms the exact change.

### Added
- **Scope and permissions** section in SKILL.md: read-only, skills folder only, no network of its own, and the single optional `clawhub` lookup.
- `metadata.openclaw.requires` and `envVars` declarations in the frontmatter.
- `.clawhubignore` so development files (`evals/`, Python cache files) aren't published.

### Security
- The `clawhub` version lookup now passes `shell=False` explicitly and only runs for names that look like ClawHub slugs, so a skill with a crafted name can't inject options into the call.

## [1.0.0] - 2026-05-23

Initial release.

### Added
- `audit` command: full health report covering conflicts, security flags, and versions.
- `conflicts` command: detects skills whose triggers overlap, via shared explicit trigger phrases and keyword Jaccard overlap (default threshold 0.20).
- `security` command: inline red-flag scan with severity ratings — remote code execution, credential exfiltration, hard-coded secrets, SSH/credential file access, reverse shells, destructive commands, `shell=True`, and `eval`/`exec`.
- `stale` command: compares installed versions against the latest ClawHub release using the `clawhub` CLI when available.
- `which "<prompt>"` command: predicts which installed skill will fire for a prompt and warns when the choice is ambiguous.
- Auto-detection of the installed-skills directory across common OpenClaw and Claude layouts, with `--skills-dir` override.
- Zero required dependencies: pure Python standard library, with optional PyYAML for the most robust frontmatter parsing.
