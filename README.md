# DevOps Pipeline Task Manager

A comprehensive DevOps implementation project featuring a fullstack task management application with complete CI/CD pipeline, Infrastructure as Code, automated configuration management, and integrated security scanning.

## 👥 Team Members

| Role | Name | GitHub | Responsibility |
|------|------|--------|----------------|
| DevOps Engineer | Team Lead | [@dianegit](https://github.com/dianegit) | GitHub/CI/CD, Docker |
| Security Engineer | Security Lead | [@yakingi](https://github.com/yakingi) | DevSecOps, Security Scanning |
| Infrastructure Engineer | Infrastructure Lead | [@nwpuu](https://github.com/nwpuu) | Terraform, Azure |
| Configuration Manager | Config Lead | [@Nyarubisi](https://github.com/Nyarubisi) | Ansible, Deployment |

## 🎯 Project Overview

This project demonstrates enterprise-level DevOps practices through the implementation of a task management application. The focus is on the DevOps pipeline, infrastructure automation, and security integration rather than complex application features.

### Features
- ✅ Create, read, update, and delete tasks
- ✅ Task status tracking (To-Do, In Progress, Done)
- ✅ User-friendly React interface
- ✅ RESTful API backend
- ✅ PostgreSQL database
- ✅ Fully containerized with Docker
- ✅ Automated CI/CD pipeline
- ✅ Cloud deployment on Azure

## 🏗️ Architecture

### Application Stack
- **Frontend:** React 18 with modern hooks
- **Backend:** Node.js with Express
- **Database:** PostgreSQL 15
- **Containerization:** Docker & Docker Compose

### DevOps Stack
- **Source Control:** GitHub with branch protection
- **CI/CD:** GitHub Actions
- **Cloud:** Microsoft Azure
- **IaC:** Terraform with Terraform Cloud
- **Config Management:** Ansible
- **Container Registry:** Azure Container Registry (ACR)
- **Compute:** Azure Virtual Machine (Ubuntu 22.04)

### Security Tools
- **SAST:** CodeQL
- **Container Scanning:** Trivy
- **Dependency Scanning:** npm audit
- **Secret Scanning:** TruffleHog
- **IaC Security:** tfsec, Checkov
- **DAST:** OWASP ZAP

## 📁 Repository Structure

```
devops-pipeline-task-manager/
├── .github/
│   ├── workflows/           # CI/CD pipeline definitions
│   ├── ISSUE_TEMPLATE/      # Issue templates
│   └── CODEOWNERS           # Code ownership rules
├── frontend/                # React application
│   ├── src/
│   ├── public/
│   ├── Dockerfile
│   └── package.json
├── backend/                 # Node.js Express API
│   ├── src/
│   ├── tests/
│   ├── Dockerfile
│   └── package.json
├── terraform/               # Infrastructure as Code
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── backend.tf
├── ansible/                 # Configuration management
│   ├── playbooks/
│   ├── roles/
│   └── inventory/
├── security/                # Security configurations
│   └── .trivyignore
├── docs/                    # Documentation
│   ├── SETUP_GUIDE.md
│   ├── ARCHITECTURE.md
│   └── DEPLOYMENT.md
├── docker-compose.yml       # Local development
├── .gitignore
├── LICENSE
└── README.md
```

## 🚀 Quick Start

### Prerequisites
- Git
- Docker Desktop
- Node.js 18+
- npm or yarn

### Local Development

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yakingi/devops-pipeline-task-manager.git
   cd devops-pipeline-task-manager
   ```

2. **Start the application:**
   ```bash
   docker-compose up --build
   ```

3. **Access the application:**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:5000
   - API Health: http://localhost:5000/health

4. **Stop the application:**
   ```bash
   docker-compose down
   ```

### Development Without Docker

See [docs/SETUP_GUIDE.md](docs/SETUP_GUIDE.md) for detailed setup instructions.

## 🌿 Branching Strategy

We follow a structured Git workflow to maintain code quality and enable parallel development:

### Branch Types
- `main` - Production-ready code (protected)
- `develop` - Integration branch for features (protected)
- `feature/*` - New features (branch from develop)
- `bugfix/*` - Bug fixes (branch from develop)
- `hotfix/*` - Emergency production fixes (branch from main)
- `release/*` - Release preparation (branch from develop)

### Workflow Process

1. **Create a feature branch:**
   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/task-description
   ```

2. **Develop and commit:**
   ```bash
   git add .
   git commit -m "feat: descriptive commit message"
   ```

3. **Push and create Pull Request:**
   ```bash
   git push origin feature/task-description
   ```
   - Open PR on GitHub targeting `develop`
   - Fill out PR template
   - Request reviews from appropriate team members

4. **Review requirements:**
   - ✅ All CI checks must pass
   - ✅ Minimum 1 peer review approval
   - ✅ Code owner approval (if applicable)
   - ✅ All conversations resolved
   - ✅ Branch up to date with target

5. **Merge:**
   - Use "Squash and merge" for feature branches
   - Delete branch after merge

### Commit Message Convention
Follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Code style changes (formatting)
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks
- `ci:` - CI/CD changes
- `perf:` - Performance improvements

## 🔄 CI/CD Pipeline

### Continuous Integration
Triggered on all pull requests and pushes to `develop` and `main`:

1. **Linting** - ESLint for code quality
2. **Unit Tests** - Jest for backend, React Testing Library for frontend
3. **Integration Tests** - API endpoint testing
4. **Security Scans** - Trivy, npm audit, secret scanning
5. **Build Verification** - Docker image builds

### Continuous Deployment
Triggered on pushes to `main`:

1. **Build & Push** - Docker images to Azure Container Registry
2. **Infrastructure** - Terraform provisions Azure resources
3. **Configuration** - Ansible configures servers
4. **Deploy** - Application deployed to Azure VM
5. **Verify** - Post-deployment health checks

## 🔒 Security Practices

- Branch protection prevents direct commits to main/develop
- Required status checks before merge
- Automated security scanning on every PR
- Container vulnerability scanning
- Infrastructure security validation (tfsec, Checkov)
- Secret management with GitHub Secrets
- Automated security updates
- Firewall and intrusion prevention (fail2ban)

## 📊 Project Management

We use GitHub Projects for task tracking:

- **Backlog** - Planned work
- **Ready** - Approved and ready to start
- **In Progress** - Currently being worked on
- **In Review** - Pull request opened
- **Done** - Merged and complete

View our project board: [DevOps Pipeline Board](../../projects)

## 🧪 Testing

### Run Tests Locally

**Backend:**
```bash
cd backend
npm test
npm test -- --coverage
```

**Frontend:**
```bash
cd frontend
npm test
npm test -- --coverage
```

**Integration Tests:**
```bash
docker-compose up -d
npm run test:integration
```

## 📚 Documentation

- [Setup Guide](docs/SETUP_GUIDE.md) - Detailed setup instructions
- [Architecture](docs/ARCHITECTURE.md) - System architecture and design decisions
- [Deployment](docs/DEPLOYMENT.md) - Deployment procedures and troubleshooting
- [Security](docs/SECURITY.md) - Security practices and policies

## 🤝 Contributing

1. Check existing issues or create a new one
2. Fork the repository
3. Create a feature branch
4. Make your changes
5. Write/update tests
6. Ensure all CI checks pass
7. Submit a pull request

See our [Contributing Guidelines](CONTRIBUTING.md) for more details.

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- African Leadership University - DevOps Course
- Open source community and tools
- Course instructors and peers

## 📞 Support

For questions or issues:
- Create an issue using our [issue templates](.github/ISSUE_TEMPLATE/)
- Contact team members via GitHub
- Check [documentation](docs/) for common solutions

---

**Project Status:** 🚧 In Development

Last Updated: November 2025