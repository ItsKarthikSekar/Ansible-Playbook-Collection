# Ansible Setup and Playbook Guide

A comprehensive guide for setting up Ansible infrastructure and running playbooks.

## 1. Infrastructure Setup

### 1.1 Control Node Setup (Ansible Server)

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install Ansible and dependencies
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible rpm tree vim python3 python3-pip sshpass git -y
```

### 1.2 Client Node Setup

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install required packages
sudo apt install sshpass tree rpm vim python3 -y
```

### 1.3 User Configuration

#### On Control Node:
```bash
# Create and configure ansible user
sudo useradd -m -s /bin/bash ansible
sudo passwd ansible
echo "ansible ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ansible

# Generate SSH key
sudo -u ansible ssh-keygen -t rsa -b 4096 -N "" -f /home/ansible/.ssh/id_rsa
sudo cat /home/ansible/.ssh/id_rsa.pub  # Copy this key for client nodes
```

#### On Client Nodes:
```bash
# Create ansible user
sudo useradd -m -s /bin/bash ansible
sudo passwd ansible
echo "ansible ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ansible

# Configure SSH access (run on control node)
ssh-copy-id -i /home/ansible/.ssh/id_rsa.pub ansible@<client-ip>
```

### 1.4 Network Configuration

```bash
# On Control Node: Edit hosts file
sudo vi /etc/hosts

# Add client entries
<client-ip> <client-hostname>

# On Control Node: Configure Ansible inventory
sudo vi /etc/ansible/hosts

# Add client groups
[webservers]
client1 ansible_host=<client-ip>
```

## 2. Running Playbooks

### 2.1 Basic Commands

```bash
# Test connectivity
ansible all -m ping

# Check syntax
ansible-playbook <playbook.yaml> --syntax-check

# Dry run
ansible-playbook <playbook.yaml> --check

# Execute playbook
ansible-playbook <playbook.yaml> -e "parameter=value"
```

### 2.2 Common Playbook Examples

```bash
# System maintenance
ansible-playbook system/update.yaml -e "target_hosts=all"
ansible-playbook system/cleanup.yaml -e "target_hosts=all"

# Service management
ansible-playbook service/restart_nginx.yaml -e "target_hosts=webservers"
ansible-playbook service/restart_php.yaml -e "target_hosts=webservers"

# Application deployment
ansible-playbook deploy/app.yaml -e "target_hosts=all version=v1.0.0"
```

## 3. Best Practices

- **Security**
  - Always use SSH keys instead of passwords
  - Regularly rotate SSH keys and passwords
  - Use vault for sensitive data
  
- **Operations**
  - Run syntax check before execution
  - Use check mode for validation
  - Keep playbooks idempotent
  - Use roles for reusable content

- **Maintenance**
  - Regular backup of control node
  - Document all custom configurations
  - Version control your playbooks
  - Test playbooks in staging first

## 4. Troubleshooting

```bash
# Check ansible version
ansible --version

# Verify connectivity
ansible all -m ping

# Debug mode
ansible-playbook playbook.yaml -vvv

# Check facts
ansible all -m setup
```

---

**Note:** Replace placeholders:
- `<client-ip>` with actual IP addresses
- `<client-hostname>` with actual hostnames
- `<playbook.yaml>` with actual playbook names