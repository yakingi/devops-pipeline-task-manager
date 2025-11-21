# GitHub Actions Workflows

This directory contains CI/CD pipeline definitions using GitHub Actions.

## Status: 🚧 To Be Created

## Planned Workflows

### ci-pipeline.yml
**Continuous Integration Pipeline**

Triggers: Pull requests and pushes to `develop` and `main`

Jobs:
- `lint-backend` - ESLint for backend code
- `lint-frontend` - ESLint for frontend code
- `test-backend` - Backend unit and integration tests
- `test-frontend` - Frontend component tests
- `security-scan` - Trivy vulnerability scanning
- `build-test` - Docker image build verification

### cd-pipeline.yml
**Continuous Deployment Pipeline**

Triggers: Pushes to `main` branch

Jobs:
- `build-and-push` - Build and push Docker images to ACR
- `deploy` - Deploy to Azure VM using Ansible
- `post-deployment` - Smoke tests and verification

### terraform.yml
**Infrastructure Pipeline**

Triggers: Changes to `terraform/` directory

Jobs:
- `terraform-plan` - Plan infrastructure changes
- `terraform-security` - Scan IaC with tfsec and Checkov
- `terraform-apply` - Apply changes (on main branch)

### security.yml
**Security Scanning Pipeline**

Triggers: PRs, pushes, and weekly schedule

Jobs:
- `dependency-scan` - npm audit for dependencies
- `sast-scan` - CodeQL static analysis
- `secret-scan` - TruffleHog secret detection
- `container-scan` - Trivy container scanning
- `iac-security` - tfsec and Ansible lint

### dast.yml
**Dynamic Application Security Testing**

Triggers: Weekly schedule and manual

Jobs:
- `zap-scan` - OWASP ZAP baseline scan

## Required Secrets

Configure these in GitHub Settings > Secrets:

### Azure Credentials
- `ARM_CLIENT_ID` - Azure service principal client ID
- `ARM_CLIENT_SECRET` - Azure service principal secret
- `ARM_SUBSCRIPTION_ID` - Azure subscription ID
- `ARM_TENANT_ID` - Azure tenant ID

### Azure Container Registry
- `ACR_NAME` - Container registry name
- `ACR_LOGIN_SERVER` - Container registry server URL
- `ACR_USERNAME` - ACR admin username
- `ACR_PASSWORD` - ACR admin password

### Deployment
- `SSH_PRIVATE_KEY` - SSH private key for VM access
- `VM_PUBLIC_IP` - Azure VM public IP address

### Database
- `DB_USER` - Database username
- `DB_PASSWORD` - Database password
- `DB_NAME` - Database name

### Terraform Cloud
- `TF_API_TOKEN` - Terraform Cloud API token

## Workflow Features

### Caching
- npm dependencies cached for faster builds
- Docker layer caching enabled
- Terraform provider caching

### Matrix Builds
- Test against multiple Node versions if needed
- Multi-architecture builds if required

### Artifacts
- Test coverage reports uploaded
- Security scan results stored
- Terraform plan outputs saved

### Notifications
- Status checks required for PR merge
- Deployment notifications
- Security vulnerability alerts

## Next Steps

1. Create `.github/workflows/` directory
2. Implement `ci-pipeline.yml`
3. Test CI pipeline with feature branch
4. Implement `cd-pipeline.yml`
5. Configure GitHub secrets
6. Implement security workflows
7. Test end-to-end pipeline
