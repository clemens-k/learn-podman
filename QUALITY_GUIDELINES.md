# Quality Standards and Guidelines

## Overview

This document outlines the quality standards and guidelines for maintaining high-quality
documentation in the Podman Learning Journey repository. Following these standards ensures
consistency, readability, and professional presentation of all content.

## Markdown Standards

### Linting Rules

This repository uses `markdownlint` with the following key rules:

- **Line Length**: Maximum 100 characters per line (except code blocks and tables)
- **Heading Style**: Use ATX-style headings (`#` syntax)
- **List Style**: Use dashes (`-`) for unordered lists
- **Code Blocks**: Use fenced code blocks with language specification
- **Blank Lines**: Required around headings and lists
- **Trailing Whitespace**: Not allowed

### Configuration Files

- `.markdownlint.json` - Markdownlint configuration
- `.vscode/settings.json` - VS Code editor settings
- `.pre-commit-config.yaml` - Pre-commit hooks configuration

## Writing Guidelines

### Structure

1. **Clear Hierarchy**: Use proper heading levels (H1 → H2 → H3)
2. **Logical Flow**: Organize content from general to specific
3. **Consistent Formatting**: Follow established patterns throughout documents

### Content Quality

1. **Accuracy**: All technical information must be verified and current
2. **Completeness**: Include all necessary context for understanding
3. **Clarity**: Write for your intended audience level
4. **Examples**: Provide working code examples when applicable

### Style Conventions

- Use **bold** for important terms and interface elements
- Use `code formatting` for file names, commands, and code snippets
- Use _italic_ for emphasis (sparingly)
- Include brief explanations for technical acronyms on first use

## Code Examples

### Requirements

- All code examples must be tested and working
- Include clear comments explaining complex operations
- Specify versions when relevant (e.g., Podman 4.0+)
- Use realistic, practical examples rather than trivial ones

### Format

```bash
# Good: Include context and explanation
podman run --name web-server -p 8080:80 -d nginx:latest

# Check if the container is running
podman ps
```

```bash
# Bad: No context or explanation
podman run nginx
```

## File Organization

### Directory Structure

```text
learn-podman/
├── docs/                 # Learning documentation
├── examples/             # Practical examples
├── scripts/              # Utility scripts
├── .github/              # GitHub workflows
├── .vscode/              # VS Code settings
├── LEARNING_PLAN.md      # Main learning plan
└── README.md             # Repository overview
```

### Naming Conventions

- Use kebab-case for file names: `migration-guide.md`
- Use descriptive names that indicate content: `podman-commands-reference.md`
- Prefix numbered sequences: `01-history-and-evolution.md`

## Quality Assurance Tools

### Automated Checks

1. **markdownlint**: Markdown style and syntax checking
2. **prettier**: Code formatting
3. **pre-commit**: Git hooks for quality checks
4. **GitHub Actions**: CI/CD pipeline for pull requests

### Manual Review Process

1. **Self-review**: Check your own work before submitting
2. **Peer review**: Have another contributor review changes
3. **Testing**: Verify all examples and commands work as described

## Development Workflow

### Before Starting Work

1. Install recommended VS Code extensions
2. Set up pre-commit hooks: `pre-commit install`
3. Review existing documentation patterns

### During Development

1. Use VS Code with configured settings for automatic formatting
2. Run linting checks regularly: `npm run lint`
3. Test all code examples in appropriate environments

### Before Committing

1. Run full quality checks: `pre-commit run --all-files`
2. Verify all links work: `markdown-link-check *.md`
3. Check spelling: `cspell "**/*.md"`

## Common Issues and Solutions

### Linting Errors

| Error | Cause                 | Solution                             |
| ----- | --------------------- | ------------------------------------ |
| MD013 | Line too long         | Break long lines at logical points   |
| MD022 | Missing blank lines   | Add blank lines around headings      |
| MD032 | Missing blank lines   | Add blank lines around lists         |
| MD047 | Missing final newline | Ensure file ends with single newline |

### Formatting Issues

- **Inconsistent indentation**: Use 2 spaces for nested lists
- **Mixed heading styles**: Always use ATX-style (`#`) headings
- **Improper code formatting**: Use fenced code blocks with language tags

## Installation and Setup

### Required Tools

```bash
# Install Node.js dependencies
npm install

# Install pre-commit
pip install pre-commit
pre-commit install

# Install VS Code extensions (automatically prompted)
code --install-extension davidanson.vscode-markdownlint
code --install-extension yzhang.markdown-all-in-one
```

### Validation Commands

```bash
# Check markdown formatting
npm run lint

# Fix auto-fixable issues
npm run lint:fix

# Check all files with pre-commit
pre-commit run --all-files

# Spell check
cspell "**/*.md"

# Link validation
find . -name "*.md" -exec markdown-link-check {} \;
```

## Contributing Guidelines

### Pull Request Requirements

- [ ] All markdown files pass linting
- [ ] All links are valid and accessible
- [ ] Code examples are tested and working
- [ ] New content follows established patterns
- [ ] Spell check passes without false positives

### Review Criteria

1. **Technical Accuracy**: Information is correct and up-to-date
2. **Clarity**: Content is understandable to the target audience
3. **Completeness**: All necessary information is included
4. **Consistency**: Follows established style and format guidelines
5. **Quality**: Professional presentation and attention to detail

## Continuous Improvement

### Regular Maintenance

- Review and update content quarterly
- Check for broken links monthly
- Update tool versions and configurations as needed
- Gather feedback from users and contributors

### Metrics and Goals

- Zero linting errors in main branch
- All external links functional
- Response time < 24 hours for issues
- Documentation coverage for all major features

---

_These guidelines are living documents that evolve with the project. Suggestions for improvements
are always welcome through issues or pull requests._
