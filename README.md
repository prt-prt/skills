# skills

Personal agent skills, one directory per skill under `skills/`, distributed to the
agentic harnesses on this machine by `bin/skills-link`.

This repo is the single source of truth for skill content. Machine configuration
(dotfiles/chezmoi) deliberately does not manage skills anymore.

## Layout

```
skills/<name>/SKILL.md   one skill per directory
bin/skills-link          idempotent distributor
```

## Distribution

| Provider | Wiring | Why |
| --- | --- | --- |
| opencode | `~/.agents/skills` → `skills/` (whole-dir symlink) | reads `~/.agents/skills` natively |
| claude | `~/.claude/skills` → `skills/` (whole-dir symlink) | personal skills location |
| codex | `~/.codex/skills/<name>` → `skills/<name>` (per-skill symlinks) | `.system/` shares the directory, so it cannot be replaced |

Run after adding or removing a skill, and on a fresh machine:

```sh
bin/skills-link            # apply
bin/skills-link --dry-run  # show what would happen
```

The script refuses to touch anything that is not already in sync with this repo
(real content that differs, unexpected entries) instead of deleting it.

On machines managed by dotfiles, `run_90-skills-link.sh` runs this on every
`chezmoi apply`, so cloning `~/skills` is the only manual step on a fresh machine.

## Adding a skill

1. `mkdir skills/<name>`, write `skills/<name>/SKILL.md` (frontmatter: `name`, `description`).
2. `bin/skills-link`.
3. Commit.

## Notes

- Codex's skill-installer writes GitHub-sourced skills into `~/.agents/skills`, which is
  this repo's checkout — review and commit what it adds. Its lock file stays machine-level
  (`~/.agents/.skill-lock.json`, managed by dotfiles).
- The `~/.agents/skills` and `~/.claude/skills` symlinks use an absolute target, so the
  repo location is fixed at `~/skills`.
