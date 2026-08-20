# Ansible Debian/Ubuntu Server Bootstrap

Bootstraps Debian and Ubuntu servers with baseline packages, a restrictive UFW policy, optional Docker Engine installation, and optional local users.

## Requirements

- Ansible 2.14 or later.
- A Debian or Ubuntu target with APT and privilege escalation available.
- Install the required collections before running the role:

  ```shell
  ansible-galaxy collection install -r requirements.yml
  ```

Docker installation uses Docker's official APT repository and requires outbound HTTPS access.

## Role Variables

| Variable | Default | Description |
| --- | --- | --- |
| `bootstrap_packages` | `ufw`, `git`, `nginx`, `fail2ban` | Packages installed from the distribution APT repositories. |
| `bootstrap_install_docker` | `true` | Installs Docker Engine and enables its service. |
| `docker_apt_architectures` | x86_64, aarch64, armv7l mappings | Maps Ansible architecture facts to Docker's APT architecture names. |
| `docker_packages` | Docker Engine packages | Docker packages installed from Docker's official repository. |
| `docker_users` | `[]` | Existing users added to the `docker` group. |
| `bootstrap_users` | `[]` | Users to create. Each item requires `name`; `groups` and `ssh_public_key` are optional. |

UFW allows TCP ports 22 and 443, then enables a deny-incoming policy. Ensure SSH uses port 22 or extend the firewall configuration before running this role remotely.

## Example Playbook

```yaml
- hosts: servers
  become: true
  roles:
    - role: ansible-debian-ubuntu-server-bootstrap
      docker_users:
        - deploy
      bootstrap_users:
        - name: deploy
          groups:
            - sudo
          ssh_public_key: "ssh-ed25519 AAAA... deploy@example.com"
```

## License

MIT-0
