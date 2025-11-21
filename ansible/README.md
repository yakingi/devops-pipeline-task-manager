# Ansible Configuration Management

This directory contains Ansible playbooks and roles for server configuration and deployment.

## Status: 🚧 To Be Configured

## Directory Structure

```
ansible/
├── playbooks/
│   └── setup-server.yml     # Main server setup playbook
├── roles/
│   ├── docker/              # Docker installation
│   ├── security/            # Security hardening
│   ├── monitoring/          # Monitoring tools
│   └── app-deploy/          # Application deployment
├── inventory/
│   ├── azure_rm.yml         # Dynamic Azure inventory
│   └── hosts                # Static inventory (for testing)
├── ansible.cfg              # Ansible configuration
└── README.md                # This file
```

## Roles

### docker
Installs and configures Docker and Docker Compose

**Tasks:**
- Install Docker CE
- Configure Docker daemon
- Add user to docker group
- Install Docker Compose
- Install Azure CLI

### security
Hardens server security

**Tasks:**
- Configure UFW firewall
- Install and configure fail2ban
- Enable automatic security updates
- Set system limits
- Update all packages

### monitoring  
Sets up monitoring tools

**Tasks:**
- Install prometheus-node-exporter
- Install sysstat
- Create monitoring scripts

### app-deploy
Deploys the application

**Tasks:**
- Login to Azure Container Registry
- Pull Docker images
- Deploy with Docker Compose
- Verify application health

## Prerequisites

1. **Ansible** installed locally
2. **Azure credentials** configured
3. **SSH access** to target servers
4. **Required collections** installed

## Setup

### 1. Install Ansible Collections

```bash
ansible-galaxy collection install azure.azcollection
ansible-galaxy collection install community.docker
pip install -r https://raw.githubusercontent.com/ansible-collections/azure/dev/requirements-azure.txt
```

### 2. Configure SSH Access

```bash
# Add SSH key to agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/devops-pipeline

# Test connection
ssh -i ~/.ssh/devops-pipeline azureuser@<VM_IP>
```

### 3. Set Environment Variables

```bash
export AZURE_CLIENT_ID="your-client-id"
export AZURE_SECRET="your-client-secret"
export AZURE_SUBSCRIPTION_ID="your-subscription-id"
export AZURE_TENANT="your-tenant-id"
```

## Usage

### Test Connectivity

```bash
cd ansible

# Test Azure inventory
ansible-inventory -i inventory/azure_rm.yml --graph

# Ping all hosts
ansible all -m ping -i inventory/azure_rm.yml
```

### Run Playbooks

```bash
# Check mode (dry run)
ansible-playbook playbooks/setup-server.yml -i inventory/azure_rm.yml --check

# Run playbook
ansible-playbook playbooks/setup-server.yml -i inventory/azure_rm.yml

# Run with verbose output
ansible-playbook playbooks/setup-server.yml -i inventory/azure_rm.yml -v
```

### Run Specific Roles

```bash
# Only run security role
ansible-playbook playbooks/setup-server.yml --tags security

# Skip specific role
ansible-playbook playbooks/setup-server.yml --skip-tags monitoring
```

## Configuration

The `ansible.cfg` file contains:
- Inventory path
- SSH settings
- Fact caching
- Privilege escalation

## Variables

Application deployment variables:
- `acr_name` - Azure Container Registry name
- `acr_login_server` - ACR login server
- `image_tag` - Docker image tag
- `db_user` - Database username
- `db_password` - Database password
- `db_name` - Database name

## Idempotency

All playbooks and roles are designed to be idempotent - safe to run multiple times without causing issues.

## Testing

```bash
# Test playbook syntax
ansible-playbook playbooks/setup-server.yml --syntax-check

# List all tasks
ansible-playbook playbooks/setup-server.yml --list-tasks

# List all hosts
ansible-playbook playbooks/setup-server.yml --list-hosts
```

## Troubleshooting

### SSH Connection Issues

```bash
# Verify SSH key
ssh -i ~/.ssh/devops-pipeline -v azureuser@<VM_IP>

# Check SSH agent
ssh-add -l
```

### Azure Authentication Issues

```bash
# Verify credentials
az account show

# Re-login
az login
```

### Inventory Issues

```bash
# Debug inventory
ansible-inventory -i inventory/azure_rm.yml --list

# Graph inventory
ansible-inventory -i inventory/azure_rm.yml --graph
```

## Next Steps

1. Create `ansible.cfg`
2. Create dynamic inventory configuration
3. Create role structures
4. Implement role tasks
5. Create main playbook
6. Test on target servers
