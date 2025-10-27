# Podman Learning Journey

A comprehensive learning repository for mastering Podman container technology, organized as a
structured educational journey with hands-on examples and best practices.

## Quick Start

### Prerequisites

- Node.js (for development tools)
- Python 3.x (for pre-commit hooks)
- Git
- VS Code (recommended)

### Setup

```bash
# Clone the repository
git clone <repository-url>
cd learn-podman

# Install dependencies
npm install

# Set up pre-commit hooks
pip install pre-commit
pre-commit install

# Install recommended VS Code extensions
# (VS Code will prompt you automatically)
```

## Repository Structure

- **docs/** - Learning documentation organized by work packages
- **examples/** - Practical examples and sample configurations
- **scripts/** - Utility scripts for common tasks
- **.github/** - GitHub Actions workflows for quality assurance
- **.vscode/** - VS Code workspace settings and extension recommendations

## Learning Path

Follow the structured learning plan outlined in [`LEARNING_PLAN.md`](LEARNING_PLAN.md):

1. **WP1**: Foundation and History
2. **WP2**: Comparative Analysis
3. **WP3**: Red Hat In-Vehicle OS Integration
4. **WP4**: Migration Strategies
5. **WP5**: OCI Containers Deep Dive
6. **WP6**: Resource Collection
7. **WP7**: Command Reference and Practical Guide
8. **WP8**: System Integration and Autostart

## Quality Standards

This repository maintains high quality standards through automated tooling:

- **Markdown Linting**: All `.md` files are checked with markdownlint
- **Spell Checking**: Content is validated with cspell
- **Link Validation**: External links are verified automatically
- **Pre-commit Hooks**: Quality checks run before each commit
- **CI/CD Pipeline**: GitHub Actions validate all pull requests

See [`QUALITY_GUIDELINES.md`](QUALITY_GUIDELINES.md) for detailed standards and contributing
guidelines.

## Development Commands

```bash
# Check markdown formatting
npm run lint

# Auto-fix formatting issues
npm run lint:fix

# Run spell check
npm run spellcheck

# Validate all links
npm run link-check

# Run all quality checks
npm run quality
```

## Contributing

1. Follow the quality guidelines in [`QUALITY_GUIDELINES.md`](QUALITY_GUIDELINES.md)
2. Ensure all quality checks pass before submitting pull requests
3. Test all code examples in appropriate environments
4. Update documentation for any new features or changes

## Resources

- [Official Podman Documentation](https://docs.podman.io/)
- [Container Tools Documentation](https://github.com/containers)

## License

This educational content is provided under the MIT License. See LICENSE file for details.

---

_This is a living learning repository. Contributions, improvements, and feedback are always
welcome!_
