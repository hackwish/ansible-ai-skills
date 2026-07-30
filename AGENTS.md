# ansible-ai-skills — agent guidance

## What this is

An Ansible role that installs AI agent skills via `npx skills add`.  
Run it with a local playbook:

```bash
ansible-playbook tests/test.yml -i tests/inventory
```

## Role structure

Standard Ansible role layout — `tasks/main.yml`, `defaults/main.yml`, `meta/main.yml`, `tests/`.

- `defaults/main.yml` defines all variables (`ai_skills_user`, `ai_skills_npx_path`, `ai_skills_global`, `ai_skills_ignore_errors`, `ai_skills` list)
- `meta/main.yml` has `min_ansible_version: 2.10`, namespace `marcelo`, role_name `ai_skills`
- `ansible.cfg` sets `roles_path = ../` — the playbook is expected one directory above the role
- `tests/test.yml` is the canonical test playbook; applies the role against `localhost`

## Non-obvious details

- **npx detection**: The role shell-falls back through `.bash_profile`, `.profile`, `.zshenv`, `.zshrc`, `.bashrc` to find `npx` (nvm/fnm/asdf users). Also sets `HOME` in the environment block so `sudo` doesn't break profile resolution. If it still fails, set `ai_skills_npx_path` explicitly.
- **`become` everywhere**: Most tasks run `become: true, become_user: "{{ ai_skills_user }}"`. The role defaults to `SUDO_USER` or `ansible_user_id`.
- **Skill list format**: Each entry has `repo` (URL or shorthand like `owner/repo`) and optionally `skill` (for multi-skill repos). See defaults for examples.
- **`--global` flag**: Defaults to `true` (`ai_skills_global: true`), meaning skills install to the user-global config dir, not per-project.

## Commits & releases

Uses semantic-release. Commit messages **must** follow conventional commits format (`feat:`, `fix:`, `chore:`, etc.). Releases auto-bump, update `CHANGELOG.md`, and tag via CI on `main`/`master`.

## Constraints

- Requires **Node.js + npm** (for `npx`) on the target host
- Requires **outbound GitHub access** — skills are fetched from GitHub repos
- Linux and macOS only (no Windows support)
- Galaxay namespace is `marcelo`, role `ai_skills`
