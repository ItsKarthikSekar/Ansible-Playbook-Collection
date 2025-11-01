# Running Ansible Playbooks

This guide explains how to run Ansible playbooks from the Ansible server.  
Always validate your playbooks with a syntax check and dry run before final execution.

---

## Steps to Run a Playbook

### 1. Login to the Ansible Server

```bash
ssh <your-ansible-server>
```

### 2. Switch to the `ansible` User

```bash
sudo su - ansible
```

### 3. Navigate to the `playbooks` Directory

```bash
cd ~/playbooks
```

### 4. Run a Syntax Check

Validate the playbook for errors before execution:

```bash
ansible-playbook <playbook.yaml> --syntax-check
```

### 5. Run a Dry Run (Check Mode)

Simulate execution without making changes:

```bash
ansible-playbook <playbook.yaml> --check
```

### 6. Run the Playbook

Execute the playbook to apply changes, passing any required variables:

```bash
ansible-playbook <playbook.yaml> -e <parameters>
```

---

## Common Playbooks & Commands

```bash
# Apply permissions
ansible-playbook apply_permissions.yaml -e "target_hosts=all"

# Clear cache
ansible-playbook clear_cache.yaml -e "target_hosts=all"

# Install PHP dependencies with Composer
ansible-playbook composer_install.yaml -e "target_hosts=all"

# Install Linux packages
ansible-playbook install_packages.yaml -e "target_hosts=all"

# Restart Supervisor
ansible-playbook restart_supervisor.yaml -e "target_hosts=all"

# Restart Nginx & PHP-FPM
ansible-playbook service_restart.yaml -e "target_hosts=all"

# Check CPU and memory usage
ansible-playbook uptime.yaml -e "target_hosts=all"

# Deploy a specific tag
ansible-playbook deploy_tag.yaml -e "target_hosts=all tag=v16.0.0"
```

---

## Notes

- Replace `<playbook.yaml>` with the actual playbook name (e.g., `site.yaml`).
- Use `--syntax-check` and `--check` before running in production.
- Ensure you are in the correct Git branch (if version-controlled) before execution.
