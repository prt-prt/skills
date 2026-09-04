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
| opencode | `~/.agents/skills/<name>` → per-skill symlink | reads `~/.agents/skills` natively |
| claude | `~/.claude/skills/<name>` → per-skill symlink | personal skills location |
| codex | `~/.codex/skills/<name>` → per-skill symlink | `.system/` shares the directory |

All three get real parent directories with per-skill child symlinks. Symlinked
*parent* directories are not reliably traversed by the harnesses' skill scanners
(opencode silently skips them), so the whole-dir-symlink variant is not used.

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

- Codex's skill-installer writes GitHub-sourced skills into `~/.agents/skills`, which
  then holds per-skill symlinks into this repo — review what the installer adds and
  copy real skill directories into `skills/` here, then re-run `bin/skills-link`.
  Its lock file stays machine-level (`~/.agents/.skill-lock.json`, managed by dotfiles).
- The per-skill symlinks use absolute targets, so the repo location is fixed at `~/skills`.
