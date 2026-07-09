# ansible-ai-skills

An Ansible role to install and configure agent skills using the Skills CLI (`npx skills add`) on Linux and macOS.

## Requirements

- **Node.js and npm**: The role relies on `npx` (included with npm v5.2.0+) to fetch and run the Skills package manager.
- **Access to GitHub**: Since the skills package manager downloads repositories directly from GitHub, active internet access is required.

## Role Variables

Variables can be customized in your playbook or group/host vars. See `defaults/main.yml` for details:

```yaml
# The user for whom to install the skills (defaults to the user executing the playbook)
ai_skills_user: "{{ ansible_env.SUDO_USER | default(ansible_user_id) }}"

# Custom path to npx (leave blank to search automatically)
ai_skills_npx_path: ""

# Whether to ignore errors during installation (defaults to false)
ai_skills_ignore_errors: false

# Whether to install skills globally so they are available in all projects (defaults to true)
ai_skills_global: true

# List of skills to install
ai_skills:
  - repo: "https://github.com/hashicorp/agent-skills"
    skill: "terraform-style-guide"
  - repo: "JuliusBrussee/caveman" # Supports shorthand without skill name
```

## Example Playbook

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
