# Ubuntu VPS Setup Ansible Playbook

[![ansible-lint](https://github.com/barremian/vps-ubuntu-playbook/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/barremian/vps-ubuntu-playbook/actions/workflows/ansible-lint.yml)

A modular Ansible playbook designed to bootstrap a fresh Ubuntu VPS with industry-standard security and developer tools.

## Prerequisites

1. **Ansible**: Installed on your local machine.
2. **SSH Key**: An existing SSH key to login to the VPS.
3. **VPS Access**: Root SSH access to the target VPS.

## Playbook Execution Flow

When you run this playbook, it performs the following steps in order:

1. **common**: Updates the apt cache, upgrades all packages, and installs essential dependencies.
2. **docker**: Removes any old Docker versions, adds Docker’s official GPG key and repository, and installs the latest Docker Engine and related tools.
3. **fail2ban**: Installs Fail2Ban, sets up its default configuration, and ensures the service is enabled and running.
4. **ufw**: Configures the Uncomplicated Firewall (UFW) to allow only specified ports (22, 80, 443) and enables the firewall with a default deny policy for incoming connections.
5. **user_management**: Creates the new user, and adds them to the sudo and docker groups.
6. **zsh**: Installs Zsh, sets it as the default shell for root and the new user, and installs Oh My Zsh with useful plugins and a custom `.zshrc`.
7. **ssh_setup**: Adds your local public SSH key for the new user, deploys a hardened SSH configuration, and restarts the SSH service to apply changes. **Note:** This disables root login.
8. **cleanup**: Removes unused packages and clears the apt cache to keep the system lean.

## Deployment Instructions

1. **Install Ansible locally**: `pip install ansible`.
2. **Update Inventory**: Put your VPS IP in `inventory.ini`.
3. **Dry Run** (check mode): Run the playbook in check mode to see what would happen without modifying the server
   ```bash
   ansible-playbook -i inventory.ini playbook.yml --check
   ```
4. **Configuration Files**: Ensure your custom `sshd_config` is in `roles/ssh_setup/files/` and `zshrc` is in `roles/zsh/files/`.
5. **Run Playbook**:
   ```bash
   ansible-playbook -i inventory.ini playbook.yml
   ```
6. **User and Path Input**: When prompted, provide the username for the new user (default: developer) and the path to your public key (default: ~/.ssh/id_rsa.pub).

## Post-Installation

After the playbook completes:

- Test Login: Open a new terminal and run `ssh <your-username>@<your-vps-ip>` (replace `<your-username>` with the username you provided, default is `developer`).
- Verify Shell: Ensure the Zsh prompt appears and plugins are active.
- Verify Docker: Run `docker ps` as the new user to ensure group permissions are correct.

## Security Note

This playbook disables root login. Do not close your current root session until you have verified you can log in as the new user via SSH.
