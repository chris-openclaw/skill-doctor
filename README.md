# Skill Doctor 🩺

The health check for the OpenClaw skills you already have installed.

Everyone talks about *finding* new skills. Nobody helps once you have twenty of them and two quietly fight over the same prompts, one is three versions behind, and one does something to your environment you never noticed. Skill Doctor is the checkup.

## What it does

- **Conflict detection** — finds skills whose triggers overlap, so you know why the agent sometimes fires the wrong one.
- **Security scan** — flags risky patterns in skill files: downloaded scripts piped into a shell, credentials or environment data sent to remote servers, embedded API tokens and cloud keys, references to private key files, destructive commands, and more.
- **Version check** — reports which skills are behind their latest ClawHub release (uses the `clawhub` CLI when present).
- **"Which fires?" prediction** — give it any prompt and it ranks which installed skill is most likely to handle it, and warns when the choice is ambiguous.

All offline and read-only. No API key. Pure Python standard library (uses PyYAML if you have it, but doesn't need it).

## Safety and scope

- Read-only: it never modifies, moves, or runs the skills it scans
- Looks only inside your skills folder
- Its only external call is an optional `clawhub info` version lookup, run without a shell
- No network access of its own; never reads credential files or secrets
- Its own rule list is written so Skill Doctor (and other scanners) don't flag it as risky; run `python skill_doctor.py security` on your skills folder and it comes back clean on itself

## Quick start

```bash
python skill_doctor.py audit                 # full report — start here
python skill_doctor.py conflicts             # just trigger overlaps
python skill_doctor.py security              # just the red-flag scan
python skill_doctor.py stale                 # installed vs latest version
python skill_doctor.py which "plan our fall festival"
```

The skills directory is auto-detected (`~/.openclaw/skills`, `~/.openclaw/extensions`, and other common locations). Point it anywhere with `--skills-dir /path/to/skills`. Add `--json` for structured output.

## Why it matters

A flagged finding is a reason to *look*, not proof of malice — Skill Doctor always shows you the exact file, line, and snippet so you can judge for yourself. The goal is a calm, honest checkup: surface the one thing worth fixing, and tell you plainly when everything's healthy.

## Requirements

- Python 3.8+
- Optional: `clawhub` CLI (for remote version checks), PyYAML (for the most robust frontmatter parsing)
