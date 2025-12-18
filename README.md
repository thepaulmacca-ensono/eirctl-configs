# Eirctl Configuration Files

[![Lint and Test](https://github.com/thepaulmacca-ensono/eirctl-configs/actions/workflows/pr.yml/badge.svg)](https://github.com/thepaulmacca-ensono/eirctl-configs/actions/workflows/pr.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Reusable task configurations for [eirctl](https://github.com/Ensono/eirctl) - enabling streamlined CI/CD workflows across Azure DevOps and GitHub pipelines.

## What is this?

**eirctl-configs** provides pre-built, production-ready task definitions that can be imported directly into your `eirctl` task runner. These configurations cover common DevOps operations including:

- **Infrastructure as Code** - Terraform formatting, validation, planning, and deployment
- **Security Scanning** - Static code analysis with Checkov
- **Pipeline Management** - Azure DevOps pipeline retention policies
- **Git Operations** - Automated tagging for releases and builds
- **Code Quality** - YAML linting and Terraform documentation generation

## Features

- 🔧 **Ready-to-use tasks** - Drop-in configurations for common DevOps workflows
- 🐳 **Containerized execution** - Tasks run in the `ensono/eir-infrastructure` container
- 🔄 **Multi-platform support** - Configurations for both Azure DevOps and GitHub Actions
- 📦 **Modular design** - Import only what you need

## Getting Started

### Prerequisites

- [eirctl](https://github.com/Ensono/eirctl/blob/main/docs/installation.md) installed
- Docker (for containerized task execution)
- Azure CLI credentials (for Azure-related tasks)

### Installation

Import configurations directly from this repository in your `eirctl.yaml`:

```yaml
import:
  - https://raw.githubusercontent.com/thepaulmacca-ensono/eirctl-configs/main/shared/contexts.yaml
  - https://raw.githubusercontent.com/thepaulmacca-ensono/eirctl-configs/main/infra/terraform.yaml
  - https://raw.githubusercontent.com/thepaulmacca-ensono/eirctl-configs/main/security/scanning.yaml
```

> [!TIP]
> To prevent breaking changes, pin to a specific version by replacing `main` with a release tag (e.g., `v0.1.0`).

Or clone locally:

```bash
git clone https://github.com/thepaulmacca-ensono/eirctl-configs.git
```

### Usage

Once imported, run tasks using `eirctl`:

```bash
# Terraform operations
eirctl run lint:terraform:format
eirctl run terraform:plan
eirctl run terraform:apply

# Security scanning
eirctl run scan:terraform:checkov

# Git tagging (Azure DevOps)
eirctl run ado:git:tag

# Git tagging (GitHub)
eirctl run github:git:tag
```

## Available Tasks

### Infrastructure (`infra/terraform.yaml`)

| Task | Description |
|------|-------------|
| `lint:terraform:format` | Perform Terraform Format Check |
| `lint:terraform:validate` | Perform Terraform Validation |
| `lint:terraform:tflint` | Perform Terraform Linting using TFLint |
| `terraform:init` | Initialise Terraform |
| `terraform:plan` | Terraform Plan |
| `terraform:apply` | Apply Terraform Plan |
| `terraform:destroy:plan` | Terraform Destroy Plan |
| `terraform:destroy:apply` | Terraform Destroy Apply |
| `terraform:ado:outputs` | Get Terraform Outputs for ADO Variable Group Import |
| `doc:terraform:generate` | Generate Terraform Module Documentation |

### Security (`security/`)

| Task | Description |
|------|-------------|
| `lint:yaml` | Perform YAML Linting using Yamllint |
| `scan:terraform:checkov` | Perform Terraform Static Code Analysis using Checkov |

### Azure DevOps (`azure_devops/`)

| Task | Description |
|------|-------------|
| `ado:buildnumber` | Update the Build Number in Azure DevOps |
| `ado:retain:pipeline` | Retain an Azure DevOps pipeline run for a configurable number of years |
| `ado:git:tag` | Tag a Commit in Azure DevOps |
| `ado:git:release:tag` | Tag a Commit in Azure DevOps for Release |

### GitHub (`github/`)

| Task | Description |
|------|-------------|
| `github:git:tag` | Tag a Commit in GitHub |

## Configuration

### Environment Variables

Common variables used by tasks:

| Variable | Description |
|----------|-------------|
| `TF_FILE_LOCATION` | Path to Terraform files |
| `TF_BACKEND_INIT` | Terraform backend initialization arguments |
| `ENVIRONMENT_NAME` | Target environment/workspace name |
| `SERVICE_NAME` | Service identifier for tagging |
| `ADO_ACCESS_TOKEN` | Azure DevOps personal access token |
| `ADO_ORGANISATION` | Azure DevOps organization name |

### Execution Context

Tasks use the shared PowerShell context (`shared/contexts.yaml`) which runs commands in the `ensono/eir-infrastructure` container:

```yaml
contexts:
  powershell:
    container:
      name: ensono/eir-infrastructure
      container_args:
        - -v ~/.azure-container:/root/.azure
      shell: pwsh
      shell_args:
        - -Command
```

## Project Structure

```text
eirctl-configs/
├── azure_devops/       # Azure DevOps specific tasks
│   ├── pipelines.yaml  # Pipeline management tasks
│   └── tagging.yaml    # Git tagging for ADO
├── github/             # GitHub specific tasks
│   └── tagging.yaml    # Git tagging for GitHub Actions
├── infra/              # Infrastructure tasks
│   └── terraform.yaml  # Terraform workflow tasks
├── security/           # Security and quality tasks
│   ├── linting.yaml    # Code linting tasks
│   └── scanning.yaml   # Security scanning tasks
├── shared/             # Shared configurations
│   └── contexts.yaml   # Execution contexts
└── test/               # Validation test configs
    └── eirctl.yaml     # Combined config for CI validation
```

## Getting Help

- 📖 [eirctl Documentation](https://github.com/Ensono/eirctl)
- 🐛 [Report Issues](https://github.com/thepaulmacca-ensono/eirctl-configs/issues)

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-task`)
3. Commit your changes (`git commit -am 'Add new task'`)
4. Push to the branch (`git push origin feature/new-task`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
