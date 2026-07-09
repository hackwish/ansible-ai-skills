# ansible-ai-skills

An Ansible role to install and configure agent skills using the Skills CLI (`npx skills add`) on Linux and macOS.

## Requirements

- **Node.js and npm**: The role relies on `npx` (included with npm v5.2.0+) to fetch and run the Skills package manager.
- **Access to GitHub**: Since the skills package manager downloads repositories directly from GitHub, active internet access is required.

## Role Variables

Variables can be customized in your playbook or group/host vars. See `defaults/main.yml` for details:

- `ai_skills_user`: The system user for whom to install the skills (defaults to the user executing the playbook, falling back to `ansible_user_id`).
- `ai_skills_npx_path`: Custom path to the `npx` executable if it's not in the target user's standard path or profiles (empty by default to search automatically).
- `ai_skills_ignore_errors`: Whether to ignore failures for individual skills and continue installing the rest (default: `false`).
- `ai_skills_global`: Whether to install the skills globally (default: `true`).

### Default Installed Skills

By default, the role installs the following skills:

1. **`terraform-style-guide`** (from `https://github.com/hashicorp/agent-skills`)
2. **`kubernetes-specialist`** (from `https://github.com/jeffallan/claude-skills`)
3. **`find-skills`** (from `https://github.com/vercel-labs/skills`)
4. **`skill-creator`** (from `https://github.com/anthropics/skills`)
5. **`systematic-debugging`** (from `https://github.com/obra/superpowers`)
6. **`code-review-excellence`** (from `https://github.com/wshobson/agents`)
7. **`frontend-design`** (from `https://github.com/anthropics/skills`)
8. **`mcp-builder`** (from `https://github.com/anthropics/skills`)
9. **`writing-plans`** (from `https://github.com/obra/superpowers`)
10. **`skill-judge`** (from `https://github.com/softaworks/agent-toolkit`)
11. **`commit-work`** (from `https://github.com/softaworks/agent-toolkit`)
12. **`daily-meeting-update`** (from `https://github.com/softaworks/agent-toolkit`)
13. **`JuliusBrussee/caveman`** (Shorthand GitHub repo)

## How-To: Add New Skills

To add new skills, you can override the `ai_skills` variable. Each skill in the list can be defined in two ways:

### 1. Specific Skill in a Repository
If a repository contains multiple skills and you want to install a specific one, specify both the `repo` and `skill` fields:
```yaml
ai_skills:
  - repo: "https://github.com/owner/repo"
    skill: "specific-skill-name"
```
This executes:
```bash
npx skills add https://github.com/owner/repo --skill specific-skill-name
```

### 2. Shorthand Repository Skill
If the repository hosts a single skill or you are using shorthand repository formatting, specify only the `repo` field:
```yaml
ai_skills:
  - repo: "owner/repo"
```
This executes:
```bash
npx skills add owner/repo
```

### Example Overriding in Playbook

```yaml
- hosts: localhost
  connection: local
  roles:
    - role: ansible-ai-skills
      vars:
        ai_skills_global: true
        ai_skills:
          # Keep a default skill
          - repo: "https://github.com/hashicorp/agent-skills"
            skill: "terraform-style-guide"
          # Add your custom skill
          - repo: "https://github.com/myusername/my-agent-skills"
            skill: "custom-coding-assistant"
          # Add shorthand repository
          - repo: "anotherowner/another-skill"
```

## Example Playbook (using defaults)

```yaml
- hosts: localhost
  connection: local
  roles:
    - role: ansible-ai-skills
      vars:
        ai_skills_ignore_errors: true
```

## License

MIT

## Author Information

marcelo
