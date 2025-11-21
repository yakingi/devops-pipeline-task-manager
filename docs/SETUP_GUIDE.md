# DevOps Pipeline Task Manager - Setup Guide

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Local Development Setup](#local-development-setup)
3. [Docker Setup](#docker-setup)
4. [Azure Setup](#azure-setup)
5. [Terraform Cloud Setup](#terraform-cloud-setup)
6. [Ansible Setup](#ansible-setup)
7. [GitHub Configuration](#github-configuration)
8. [Troubleshooting](#troubleshooting)

---

## Prerequisites

### Required Software

#### 1. Git
**Windows:**
```powershell
winget install Git.Git
```

**Verify installation:**
```bash
git --version
```

#### 2. Node.js 18+
**Windows:**
```powershell
winget install OpenJS.NodeJS.LTS
```

**Verify installation:**
```bash
node --version
npm --version
```

#### 3. Docker Desktop
**Windows:**
- Download from [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop)
- Install and start Docker Desktop
- Enable WSL 2 backend if on Windows

**Verify installation:**
```bash
docker --version
docker-compose --version
```

#### 4. Azure CLI (for infrastructure deployment)
**Windows:**
```powershell
winget install Microsoft.AzureCLI
```

**Verify installation:**
```bash
az --version
```

#### 5. Terraform CLI (for infrastructure management)
**Windows:**
```powershell
winget install Hashicorp.Terraform
```

**Verify installation:**
```bash
terraform --version
```

#### 6. Ansible (for configuration management)
**Windows (via WSL):**
```bash
wsl --install
# Inside WSL
sudo apt update
sudo apt install ansible
```

**Verify installation:**
```bash
ansible --version
```

### Recommended Tools
- **VS Code** - IDE with extensions for Docker, Terraform, Ansible
- **Postman** - API testing
- **Azure Storage Explorer** - Azure resource management

---

## Local Development Setup

### 1. Clone the Repository

```bash
# Clone the repository
git clone https://github.com/yakingi/devops-pipeline-task-manager.git
cd devops-pipeline-task-manager

# Create develop branch
git checkout -b develop
git push -u origin develop
```

### 2. Backend Setup (Node.js/Express)

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Edit .env with your configuration
# DATABASE_URL=postgresql://user:password@localhost:5432/taskmanager
# PORT=5000
# NODE_ENV=development

# Run database migrations (once application is developed)
npm run migrate

# Start development server
npm run dev
```

Backend will run on: http://localhost:5000

### 3. Frontend Setup (React)

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Edit .env with your configuration
# REACT_APP_API_URL=http://localhost:5000

# Start development server
npm start
```

Frontend will run on: http://localhost:3000

### 4. Database Setup (PostgreSQL)

**Using Docker (Recommended):**
```bash
# Start PostgreSQL container
docker run --name taskmanager-db \
  -e POSTGRES_PASSWORD=devpassword \
  -e POSTGRES_USER=devuser \
  -e POSTGRES_DB=taskmanager \
  -p 5432:5432 \
  -d postgres:15-alpine
```

**Using local PostgreSQL installation:**
```bash
# Create database
psql -U postgres
CREATE DATABASE taskmanager;
CREATE USER devuser WITH PASSWORD 'devpassword';
GRANT ALL PRIVILEGES ON DATABASE taskmanager TO devuser;
```

### 5. Run Tests

```bash
# Backend tests
cd backend
npm test
npm run test:coverage

# Frontend tests
cd frontend
npm test
npm run test:coverage
```

---

## Docker Setup

### 1. Using Docker Compose (Easiest)

```bash
# Build and start all services
docker-compose up --build

# Run in detached mode
docker-compose up -d --build

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Stop and remove volumes (clean slate)
docker-compose down -v
```

### 2. Individual Container Builds

```bash
# Build backend
docker build -t taskmanager-backend ./backend

# Build frontend
docker build -t taskmanager-frontend ./frontend

# Run backend
docker run -p 5000:5000 \
  -e DATABASE_URL=postgresql://user:pass@host:5432/db \
  taskmanager-backend

# Run frontend
docker run -p 3000:80 taskmanager-frontend
```

### 3. Verify Docker Setup

```bash
# Check running containers
docker ps

# Check container health
docker inspect taskmanager-backend | grep Health

# Access container logs
docker logs taskmanager-backend

# Execute commands in container
docker exec -it taskmanager-backend /bin/sh
```

---

## Azure Setup

### 1. Create Azure Account

1. Visit [Azure for Students](https://azure.microsoft.com/en-us/free/students/)
2. Sign up with your university email
3. Get $100 free credit

### 2. Azure CLI Login

```bash
# Login to Azure
az login

# Set subscription (if you have multiple)
az account list --output table
az account set --subscription "subscription-id"

# Verify login
az account show
```

### 3. Create Service Principal

```bash
# Create service principal for Terraform
az ad sp create-for-rbac \
  --name "devops-pipeline-sp" \
  --role="Contributor" \
  --scopes="/subscriptions/YOUR_SUBSCRIPTION_ID"
```

**Save the output:**
```json
{
  "appId": "00000000-0000-0000-0000-000000000000",
  "displayName": "devops-pipeline-sp",
  "password": "your-client-secret",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

You'll need these values for:
- `ARM_CLIENT_ID` = appId
- `ARM_CLIENT_SECRET` = password
- `ARM_TENANT_ID` = tenant
- `ARM_SUBSCRIPTION_ID` = your subscription ID

---

## Terraform Cloud Setup

### 1. Create Terraform Cloud Account

1. Visit [app.terraform.io](https://app.terraform.io/)
2. Sign up for free account
3. Create organization (e.g., "devops-alu-2025")

### 2. Create Workspace

1. Click "New Workspace"
2. Choose "CLI-driven workflow"
3. Name: `devops-pipeline-infrastructure`
4. Click "Create workspace"

### 3. Configure Variables

In workspace settings, add these variables:

**Environment Variables (mark as sensitive):**
- `ARM_CLIENT_ID` - Azure service principal client ID
- `ARM_CLIENT_SECRET` - Azure service principal secret (**sensitive**)
- `ARM_SUBSCRIPTION_ID` - Azure subscription ID
- `ARM_TENANT_ID` - Azure tenant ID

**Terraform Variables:**
- `project_name` - "devopspipeline"
- `environment` - "dev"
- `location` - "East US"
- `ssh_public_key` - Your SSH public key (**sensitive**)

### 4. Generate SSH Key

```bash
# Generate SSH key pair
ssh-keygen -t rsa -b 4096 -f ~/.ssh/devops-pipeline

# Display public key (copy this to Terraform Cloud)
cat ~/.ssh/devops-pipeline.pub
```

### 5. Login to Terraform Cloud

```bash
# Login via CLI
terraform login

# This will open browser for authentication
# Follow the prompts
```

### 6. Update Backend Configuration

Edit `terraform/backend.tf`:
```hcl
terraform {
  cloud {
    organization = "your-org-name"  # Change this
    
    workspaces {
      name = "devops-pipeline-infrastructure"
    }
  }
}
```

---

## Ansible Setup

### 1. Install Ansible Collections

```bash
# Install required collections
ansible-galaxy collection install azure.azcollection
ansible-galaxy collection install community.docker

# Install Azure SDK for Python
pip install -r https://raw.githubusercontent.com/ansible-collections/azure/dev/requirements-azure.txt
```

### 2. Configure Ansible

The `ansible/ansible.cfg` file is already configured. Verify it exists:

```bash
cat ansible/ansible.cfg
```

### 3. Test Ansible Connection

```bash
# Test local connection
ansible localhost -m ping

# Test Azure inventory (after infrastructure is provisioned)
cd ansible
ansible-inventory -i inventory/azure_rm.yml --graph
```

### 4. Azure Authentication for Ansible

Ansible will use the same Azure credentials. Set environment variables:

```bash
export AZURE_CLIENT_ID="your-client-id"
export AZURE_SECRET="your-client-secret"
export AZURE_SUBSCRIPTION_ID="your-subscription-id"
export AZURE_TENANT="your-tenant-id"
```

---

## GitHub Configuration

### 1. Invite Team Members

1. Go to repository Settings > Collaborators
2. Add team members:
   - @dianegit
   - @yakingi
   - @nwpuu
   - @Nyarubisi
3. Give "Write" permissions

### 2. Configure Branch Protection

**For `main` branch:**

1. Go to Settings > Branches > Add rule
2. Branch name pattern: `main`
3. Enable:
   - ✅ Require a pull request before merging
   - ✅ Require approvals (1)
   - ✅ Dismiss stale pull request approvals
   - ✅ Require review from Code Owners
   - ✅ Require status checks to pass
   - ✅ Require branches to be up to date
   - ✅ Require conversation resolution
   - ✅ Do not allow bypassing

**Repeat for `develop` branch**

### 3. Set Up GitHub Secrets

Go to Settings > Secrets and variables > Actions > New repository secret

Add these secrets:
- `ACR_NAME` - Azure Container Registry name
- `ACR_LOGIN_SERVER` - ACR login server URL
- `ACR_USERNAME` - ACR admin username
- `ACR_PASSWORD` - ACR admin password
- `ARM_CLIENT_ID` - Azure service principal client ID
- `ARM_CLIENT_SECRET` - Azure service principal secret
- `ARM_SUBSCRIPTION_ID` - Azure subscription ID
- `ARM_TENANT_ID` - Azure tenant ID
- `SSH_PRIVATE_KEY` - SSH private key for VM access
- `VM_PUBLIC_IP` - Azure VM public IP (after provisioning)
- `DB_USER` - Database username
- `DB_PASSWORD` - Database password
- `DB_NAME` - Database name
- `TF_API_TOKEN` - Terraform Cloud API token

### 4. Create GitHub Project

1. Go to Projects > New project
2. Choose "Board" view
3. Name: "DevOps Pipeline Implementation"
4. Add columns: Backlog, Ready, In Progress, In Review, Done
5. Configure automation in settings

---

## Troubleshooting

### Docker Issues

**Problem:** Docker daemon not running
```bash
# Windows: Start Docker Desktop
# Linux: 
sudo systemctl start docker
```

**Problem:** Port already in use
```bash
# Find process using port
netstat -ano | findstr :5000  # Windows
lsof -i :5000  # Linux/Mac

# Kill process
taskkill /PID <PID> /F  # Windows
kill -9 <PID>  # Linux/Mac
```

**Problem:** Permission denied
```bash
# Add user to docker group
sudo usermod -aG docker $USER
# Log out and log back in
```

### Database Issues

**Problem:** Cannot connect to PostgreSQL
```bash
# Check if PostgreSQL is running
docker ps | grep postgres

# Check logs
docker logs taskmanager-db

# Verify connection string
echo $DATABASE_URL
```

**Problem:** Database schema issues
```bash
# Reset database
docker-compose down -v
docker-compose up -d database
# Wait for database to be ready
npm run migrate
```

### Terraform Issues

**Problem:** Authentication failed
```bash
# Verify Azure credentials
az account show

# Re-login
az login

# Verify Terraform Cloud token
terraform login
```

**Problem:** State lock error
```bash
# Force unlock (use carefully!)
terraform force-unlock LOCK_ID
```

### Ansible Issues

**Problem:** SSH connection failed
```bash
# Verify SSH key
ssh -i ~/.ssh/devops-pipeline azureuser@VM_IP

# Check SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/devops-pipeline
```

**Problem:** Azure authentication failed
```bash
# Verify environment variables
echo $AZURE_CLIENT_ID
echo $AZURE_SUBSCRIPTION_ID

# Test Azure connection
az account show
```

### Node.js/npm Issues

**Problem:** Module not found
```bash
# Clear npm cache
npm cache clean --force

# Remove node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

**Problem:** Version conflicts
```bash
# Use specific Node version with nvm
nvm install 18
nvm use 18
```

### General Debugging

**View all running services:**
```bash
docker-compose ps
```

**Check application logs:**
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f backend
```

**Restart specific service:**
```bash
docker-compose restart backend
```

**Clean Docker system:**
```bash
# Remove all stopped containers
docker container prune

# Remove unused images
docker image prune

# Remove everything (careful!)
docker system prune -a
```

---

## Next Steps

1. ✅ Complete local setup following this guide
2. ✅ Verify all services run via Docker Compose
3. ✅ Set up Azure account and service principal
4. ✅ Configure Terraform Cloud workspace
5. ✅ Set up GitHub branch protection
6. ✅ Add GitHub secrets
7. ⏭️ Start developing application features
8. ⏭️ Implement CI/CD pipelines
9. ⏭️ Deploy infrastructure with Terraform
10. ⏭️ Configure servers with Ansible

---

## Support

If you encounter issues:
1. Check this troubleshooting guide
2. Search existing GitHub issues
3. Create new issue using appropriate template
4. Ask team members on communication channel
5. Consult official documentation links

## Useful Links

- [Node.js Documentation](https://nodejs.org/docs/)
- [React Documentation](https://react.dev/)
- [Express Documentation](https://expressjs.com/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Terraform Documentation](https://www.terraform.io/docs)
- [Ansible Documentation](https://docs.ansible.com/)
- [Azure Documentation](https://docs.microsoft.com/azure)
- [GitHub Actions Documentation](https://docs.github.com/actions)
