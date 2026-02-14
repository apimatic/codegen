# CLAUDE.md

## Project Overview

This is APImatic's **codegen** repository — a centralized configuration repository that stores linting, formatting, and static analysis tool configurations for code samples across multiple programming languages. It is not a runnable application; it contains only configuration files consumed by other projects.

## Repository Structure

```
codegen/
├── README.md
├── CLAUDE.md
└── code-sample-analysis-tools/
    └── config/
        ├── dotnet/       # .editorconfig — C#/VB.NET formatting and style rules
        ├── java/         # checkstyle.xml — Java code style (Sun conventions)
        ├── php/          # phpcs-ruleset.xml — PHP CodeSniffer (PSR-12)
        ├── python/       # ruff.toml + mypy.ini — Ruff linter and mypy type checker
        ├── ruby/         # .rubocop.yml — RuboCop linter
        └── typescript/   # .eslintrc.json — ESLint with TypeScript support
```

Each language directory is self-contained with no cross-language dependencies.

## Supported Languages and Tools

| Language       | Tool(s)              | Config File(s)              |
|----------------|----------------------|-----------------------------|
| TypeScript/JS  | ESLint               | `.eslintrc.json`            |
| Python         | Ruff, mypy           | `ruff.toml`, `mypy.ini`     |
| Java           | Checkstyle           | `checkstyle.xml`            |
| C#/VB.NET      | EditorConfig         | `.editorconfig`             |
| PHP            | PHP CodeSniffer      | `phpcs-ruleset.xml`         |
| Ruby           | RuboCop              | `.rubocop.yml`              |

## Build / Test / Lint

There are **no build steps, test suites, or CI pipelines** in this repository. The configurations are static files meant to be consumed by other projects (via submodule, copy, or package reference).

## Key Conventions

### Commit Messages

Follow **Conventional Commits** format:

```
type(scope): subject
```

Examples from history:
- `ci(typescript): add eslint tool configuration`
- `add code sample analysis tools configurations`

Use a lowercase subject without a trailing period.

### Adding a New Language Configuration

1. Create a new directory under `code-sample-analysis-tools/config/<language>/`.
2. Add the tool-specific config file(s) for that language.
3. Commit with a descriptive conventional-commit message.

### Modifying Existing Configurations

- Each config file is self-documented via its native format (XML comments, TOML comments, YAML comments, etc.).
- Changes should be intentional and well-scoped — these configs are shared across projects.

## Notable Configuration Choices

- **Python (Ruff):** Selects ALL rule categories, then explicitly ignores specific rules (D103, E501, ANN201, etc.). Very strict by default.
- **Python (mypy):** Disables strict checks for assignments, indexing, and attributes. Ignores missing imports from APImatic SDK packages (`apimatic_core`, `apimatic_core_interfaces`, etc.).
- **TypeScript (ESLint):** Extends recommended + TypeScript-recommended. Disables `no-unused-vars`, `no-explicit-any`, and `no-irregular-whitespace`.
- **Java (Checkstyle):** Based on Sun conventions. Ignores method/file length violations. Enforces Javadoc and naming conventions.
- **Ruby (RuboCop):** Very permissive — most style and metrics checks are disabled.
- **PHP (PHPCS):** Enforces PSR-12. Disallows long array syntax (`array()`) in favor of `[]`.
- **.NET (EditorConfig):** Comprehensive C#/VB.NET rules. PascalCase constants, 4-space indent, UTF-8-BOM encoding. Disables many IDE0xxx and CA diagnostic rules.
