# stillrunning Security Skill for Claude

> Claude Code skill — automatic supply chain protection against 200,000+ verified malicious packages from 8 threat intelligence sources.

## Install in Claude Code

```bash
mkdir -p ~/.claude/skills/
git clone https://github.com/johhnyg/stillrunning-skill \
  ~/.claude/skills/stillrunning-skill
```

Or via MCP:

```bash
claude mcp add stillrunning -- stillrunning mcp
```

## Supported package managers

pip, uv, poetry, pdm, pipenv, conda, pixi, npm, bun, pnpm

## What happens after install

Every time you or Claude Code runs a package install command:

- **CLEAN** - installs silently
- **SUSPICIOUS** - warns you, asks to confirm  
- **MALICIOUS** - hard blocked, links to advisory

## Public API (no token needed)

10 free checks/day:

```
https://stillrunning.io/api/check-package?name=requests&ecosystem=pip
```

## Get full protection

```bash
pip install stillrunning
stillrunning --setup
```

Token saved to `~/.stillrunning/config.yaml`

## Browse threats

[stillrunning.io/security-advisories](https://stillrunning.io/security-advisories)

---

[stillrunning.io](https://stillrunning.io) | `pip install stillrunning` | [@bit_bot9000](https://x.com/bit_bot9000)
