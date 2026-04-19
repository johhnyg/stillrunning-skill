# stillrunning Security Skill for Claude

Automatically checks every pip and npm package against the stillrunning.io threat database. Blocks supply chain attacks before they execute.

## Install in Claude Code

```bash
mkdir -p ~/.claude/skills/
git clone https://github.com/johhnyg/stillrunning-skill \
  ~/.claude/skills/stillrunning-skill
```

## Install in Claude.ai

1. Download repo as ZIP
2. Claude.ai > Settings > Capabilities > Skills > Upload
3. Upload stillrunning-skill/ folder
4. Toggle on stillrunning-security

## What happens after install

Every time you or Claude Code runs `pip install`, `npm install`, or any package command:

- **CLEAN** - installs silently
- **SUSPICIOUS** - warns you, asks to confirm
- **DANGEROUS** - hard blocked, explains why

## Public API (no token needed)

10 free checks/day:

```
https://stillrunning.io/api/check-package?name=requests
```

## Get full protection

```bash
pip install stillrunning
stillrunning --setup
```

Token saved to `~/stillrunning.yaml`

---

[stillrunning.io](https://stillrunning.io) | `pip install stillrunning` | [@bit_bot9000](https://x.com/bit_bot9000)
