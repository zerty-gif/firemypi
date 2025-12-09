# Contributing to FireMyPi

Thank you for your interest in contributing to FireMyPi! This document provides guidelines and instructions for contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Prerequisites](#prerequisites)
- [Development Setup](#development-setup)
- [Making Changes](#making-changes)
- [Submitting Pull Requests](#submitting-pull-requests)
- [Code Style](#code-style)
- [Testing](#testing)

## Code of Conduct

Please be respectful and constructive in all interactions. We welcome contributions from everyone regardless of experience level.

## Getting Started

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/firemypi.git
   cd firemypi
   ```
3. Add the upstream repository as a remote:
   ```bash
   git remote add upstream https://github.com/zerty-gif/firemypi.git
   ```

## Prerequisites

Before contributing, ensure you have the following installed:

- **Ansible** (version 2.9 or later) - For running playbooks
- **ShellCheck** - For linting shell scripts
- **Bash** (version 4.0 or later) - For running scripts

### Installing Prerequisites

**On Debian/Ubuntu:**
```bash
sudo apt update
sudo apt install ansible shellcheck apache2-utils openssl pwgen u-boot-tools
```

**On macOS:**
```bash
brew install ansible shellcheck pwgen u-boot-tools
```

## Development Setup

1. Accept the license by running:
   ```bash
   ./accept-license.sh
   ```

2. Verify your environment:
   ```bash
   ./show-build-environment.sh
   ```

3. Review the configuration files in the `config/` directory

## Making Changes

1. Create a new branch for your changes:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Make your changes following the [Code Style](#code-style) guidelines

3. Test your changes locally

4. Commit your changes with a clear, descriptive message:
   ```bash
   git commit -m "Add feature: description of changes"
   ```

## Submitting Pull Requests

1. Push your changes to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

2. Open a Pull Request on GitHub

3. Provide a clear description of:
   - What changes you made
   - Why you made them
   - How to test them

4. Wait for review and address any feedback

## Code Style

### Shell Scripts

- Use `#!/bin/bash` as the shebang
- Follow [ShellCheck](https://www.shellcheck.net/) recommendations
- Use meaningful variable names
- Quote variables to prevent word splitting: `"${variable}"`
- Use functions for reusable code
- Add comments for complex logic

### Ansible Playbooks

- Use YAML format consistently
- Follow Ansible best practices for playbook structure
- Use meaningful task names
- Document variables and their purpose

### Jinja2 Templates

- Use clear variable names
- Add comments for complex logic
- Follow the existing template patterns in `resource/`

## Testing

### Running Linters

Before submitting a PR, run shellcheck on all shell scripts:

```bash
shellcheck *.sh
```

### Manual Testing

1. Set up a test configuration using `--test` flag
2. Verify builds complete successfully
3. Test on actual Raspberry Pi hardware when possible

## Questions?

If you have questions or need help, please open an issue on GitHub.

## License

By contributing to FireMyPi, you agree that your contributions will be licensed under the same [CC BY-NC-ND 4.0](LICENSE) license as the project.
