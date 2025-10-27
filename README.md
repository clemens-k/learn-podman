# Podman Learning Journey

A comprehensive learning repository for mastering Podman container technology, focusing on its foundation, history, and evolution in the container ecosystem.

## What You'll Learn

This repository currently provides:

- **Foundation and History**: Complete overview of Podman's origins at Red Hat, evolution from Docker alternative to modern container runtime, and position in the container ecosystem
- **Technical Timeline**: Detailed timeline of major releases and features from early development to current state
- **Community and Contributors**: Understanding of the key people and organizations behind Podman
- **Ecosystem Relationships**: How Podman relates to CRI-O, Buildah, and other container tools

## Repository Structure

- **01-history-and-evolution.md** - Complete foundation document covering Podman's journey
- **LEARNING_PLAN.md** - [Inernal] Detailed work package structure and implementation tracking
- **QUALITY_GUIDELINES.md** - [Internal] Documentation standards and contributing guidelines

## Getting Started

1. Start with [`01-history-and-evolution.md`](01-history-and-evolution.md) to understand Podman's foundation
2. Review [`LEARNING_PLAN.md`](LEARNING_PLAN.md) to see the overall learning journey structure
3. Follow quality guidelines in [`QUALITY_GUIDELINES.md`](QUALITY_GUIDELINES.md) if contributing

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

## Disclaimer

Most of the information presented in this repo is collected and summarized by
Claude Sonnet 4, so please take any information with a grain of salt. If you find wrong
information, please create an issue or pull request!

---

_This is a living learning repository. Contributions, improvements, and feedback are always
welcome!_
