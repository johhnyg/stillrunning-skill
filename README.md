# stillrunning Security Skill for Claude

Automatically checks every pip and npm package against
the stillrunning.io threat database before installing.
Blocks supply chain attacks before they execute.

## What it does
- Blocks DANGEROUS packages (score 80+) — hard block, no override
- Warns on SUSPICIOUS packages (score 50-79) — ask user to confirm
- Silent on CLEAN packages — no interruption
- Works with: pip, pip3, npm, npx, poetry, requirements.txt, package.json
- Checks imports via stillrunning.hook

## Install in Claude Code
```bash
mkdir -p ~/.claude/skills/
git clone https://github.com/johhnyg/stillrunning-skill ~/.claude/skills/stillrunning-skill
```

## Install in Claude.ai
1. Download this repo as ZIP
2. Claude.ai → Settings → Capabilities → Skills → Upload
3. Upload the stillrunning-skill/ folder (zipped)
4. Toggle on stillrunning-security

## Get your API token (free)
```bash
pip install stillrunning
stillrunning --setup
```
Token saved automatically to ~/stillrunning.yaml

## Public API — no token needed
50 free checks/day:
```
https://stillrunning.io/api/check-package?name=requests&version=2.31.0
```

## Why this exists
In early 2026, North Korean state hackers published 1,700+
malicious packages to npm and PyPI. Traditional antivirus found nothing.
stillrunning catches them before they install.

---

stillrunning.io | pip install stillrunning | @bit_bot9000
