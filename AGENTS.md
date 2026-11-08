# AGENTS.md — rules for publishing skills to sevruk-ai-skills

Repository: https://github.com/sevruk-ai/sevruk-ai-skills (public, MIT license).
Local copy: `~/GitHub/sevruk-ai-skills`.
This file is single-source: edits in the project folder change the repository file (symlink).

## Publishing a new skill

1. The skill source lives strictly in its own project subfolder: `~/Documents/Claude/Projects/Agent Skills/<name>/`. Publish from project subfolders, not from Hermes (do not touch `~/.hermes/skills`).
2. Copy it to `skills/<name>/` in the repository (including all subfolders: references/, templates/, scripts/).
3. **Mandatory cleanup before publishing** — remove/replace from SKILL.md and all nested files:
   - passwords, tokens, keys, server IPs, logins;
   - the owner's personal and work data (name, projects, client domains);
   - links to closed services and internal files.
   Only cleaned skills may be published.
4. Update the table in README.md: add a row "skill name — purpose".
5. Commit and push: `git add -A && git commit -m "Add skill: <name>" && git push`.

## Rules

- Commit signature: sevruk-ai (set globally, do not change).
- Publish SKILL.md in English; README and descriptions in English.
- One skill = one folder with SKILL.md. One commit = one skill (or one fix).
- No secrets in commit history: if something slipped in, tell the owner immediately.
