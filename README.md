# Ubuntu VPS Setup Ansible Playbook

A modular Ansible playbook designed to bootstrap a fresh Ubuntu VPS with industry-standard security and developer tools.

## Prerequisites

1. **Ansible**: Installed on your local machine.
2. **SSH Key**: An existing SSH key to login to the VPS.
3. **VPS Access**: Root SSH access to the target VPS.

## Deployment Instructions

1. **Install Ansible locally**: `pip install ansible`.
2. **Update Inventory**: Put your VPS IP in `inventory.ini`.
3. **Dry Run** (check mode): Run the playbook in check mode to see what would happen without modifying the server
   ```bash
   ansible-playbook -i inventory.ini playbook.yml --check
   ```
4. **Configuration Files**: Ensure your custom `sshd_config` and `zshrc` are in the `files/` folder.
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

This playbook disables root login. Do not close your current root session until you have verified you can log in as developer via SSH.
