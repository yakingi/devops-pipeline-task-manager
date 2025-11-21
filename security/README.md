# Security Configurations

This directory contains security-related configurations for the DevOps pipeline.

## Files

### .trivyignore
Configuration file for Trivy vulnerability scanner to ignore specific CVEs with justification.

## Security Scanning Tools

Our pipeline includes:
- **Trivy** - Container and filesystem vulnerability scanning
- **npm audit** - Dependency vulnerability scanning
- **CodeQL** - Static application security testing (SAST)
- **TruffleHog** - Secret scanning
- **tfsec** - Terraform security scanning
- **Checkov** - Infrastructure as Code security
- **OWASP ZAP** - Dynamic application security testing (DAST)

## Security Policies

See [docs/SECURITY.md](../docs/SECURITY.md) for comprehensive security practices.
