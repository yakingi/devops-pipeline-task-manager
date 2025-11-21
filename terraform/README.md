# Terraform Infrastructure as Code

This directory contains Terraform configurations for provisioning Azure infrastructure.

## Status: 🚧 To Be Configured

## Infrastructure Components

The Terraform configuration will provision:

- **Resource Group** - Container for all Azure resources
- **Virtual Network** - Network infrastructure
- **Network Security Group** - Firewall rules
- **Virtual Machine** - Ubuntu 22.04 LTS for application hosting
- **Public IP Address** - Static IP for VM access
- **Container Registry** - Azure Container Registry for Docker images
- **SSH Key** - Secure VM access

## Files

- `main.tf` - Main infrastructure definitions
- `variables.tf` - Input variables
- `outputs.tf` - Output values
- `backend.tf` - Terraform Cloud backend configuration
- `terraform.tfvars.example` - Example variable values

## Prerequisites

1. **Azure Account** with active subscription
2. **Azure Service Principal** created
3. **Terraform Cloud** account and workspace
4. **SSH Key Pair** generated

## Setup Steps

### 1. Create Azure Service Principal

```bash
az ad sp create-for-rbac \
  --name "devops-pipeline-sp" \
  --role="Contributor" \
  --scopes="/subscriptions/YOUR_SUBSCRIPTION_ID"
```

### 2. Configure Terraform Cloud

1. Create organization and workspace
2. Add environment variables:
   - `ARM_CLIENT_ID`
   - `ARM_CLIENT_SECRET` (sensitive)
   - `ARM_SUBSCRIPTION_ID`
   - `ARM_TENANT_ID`
3. Add Terraform variables:
   - `ssh_public_key` (sensitive)

### 3. Update Backend Configuration

Edit `backend.tf` with your organization name:
```hcl
terraform {
  cloud {
    organization = "your-org-name"
    workspaces {
      name = "devops-pipeline-infrastructure"
    }
  }
}
```

### 4. Initialize Terraform

```bash
cd terraform
terraform login
terraform init
```

## Usage

```bash
# Validate configuration
terraform validate

# Plan infrastructure changes
terraform plan

# Apply changes
terraform apply

# View outputs
terraform output

# Destroy infrastructure (when done)
terraform destroy
```

## Outputs

After applying, you'll get:
- `vm_public_ip` - VM public IP address
- `acr_login_server` - Container registry URL
- `acr_name` - Container registry name
- `resource_group_name` - Resource group name

## Security Considerations

- SSH key is managed securely
- Network Security Group limits inbound traffic
- Container Registry uses admin authentication
- All resources tagged for tracking

## Cost Estimation

Expected monthly costs (approximate):
- VM (Standard_B2s): ~$30
- Container Registry (Basic): ~$5
- Public IP: ~$3
- Storage: ~$2

**Total: ~$40/month**

## Next Steps

1. Create `backend.tf` file
2. Create `main.tf` with resource definitions
3. Create `variables.tf` 
4. Create `outputs.tf`
5. Create `terraform.tfvars.example`
6. Test with `terraform plan`
7. Apply infrastructure
