# sevruk-ai-skills

A collection of skills for AI agents in the open **Agent Skills (SKILL.md)** format. Compatible with Hermes, Claude Code, Codex CLI, and other agents that support the standard.

## Install

Hermes:

```
hermes skills install sevruk-ai/sevruk-ai-skills/skills/<skill-name>
```

Claude Code / Codex (via npx skills):

```
npx skills add https://github.com/sevruk-ai/sevruk-ai-skills --skill <skill-name>
```

Claude Code (built-in plugin marketplace):

```
/plugin marketplace add sevruk-ai/sevruk-ai-skills
/plugin install <skill-name>@sevruk-ai-skills
```

Or manually: copy the skill folder into your agent's skills directory.

## Skills

| Skill | Purpose |
| --- | --- |
| [always-check-current-date](skills/always-check-current-date/SKILL.md) | Verify the current date and time before every reply and stamp each answer. |

## License

MIT. Use freely; if you improve something, send a pull request.
