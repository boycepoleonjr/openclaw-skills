# openclaw-skills

Homegrown [OpenClaw](https://openclaw.ai) skill library. Every skill here is written from scratch — no third-party code, no trust required.

## Why

After the [malicious skills incident](https://clawhub.ai) on ClawhHub, we decided to build our own. Every skill is:

- **Auditable** — pure SKILL.md with optional scripts, all readable in 5 minutes
- **Dependency-free** — no npm packages, no pip installs, no supply chain risk
- **Battle-tested** — used daily before publishing

## Skills

| Skill | Description |
|-------|-------------|
| [deslop](skills/deslop/) | Detect and remove AI writing patterns. 24 pattern categories, vocabulary tiers, statistical signals. |

## Install

Copy the skill folder into your OpenClaw skills directory:

```bash
# Clone and symlink
git clone https://github.com/boycepoleonjr/openclaw-skills.git
ln -s $(pwd)/openclaw-skills/skills/deslop ~/.openclaw/skills/deslop
```

Or just copy the SKILL.md into your workspace `skills/` folder.

## Contributing

Open an issue or PR. Keep skills lean — if your SKILL.md is over 500 lines, split into references.

## License

MIT
