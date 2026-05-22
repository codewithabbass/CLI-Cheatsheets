# Contributing to Commands Guide

Thank you for your interest in contributing! This guide explains how to add commands, fix errors, and submit improvements.

---

## Table of Contents

- [How to Contribute](#how-to-contribute)
- [File Structure](#file-structure)
- [Command Format](#command-format)
- [Adding a New Category](#adding-a-new-category)
- [Submitting a Pull Request](#submitting-a-pull-request)
- [Style Guidelines](#style-guidelines)

---

## How to Contribute

There are several ways to help:

- **Add missing commands** — found a useful command that isn't listed? Add it!
- **Fix errors** — spotted a typo, wrong flag, or outdated syntax? Open a PR.
- **Improve descriptions** — make explanations clearer or more helpful.
- **Add a new category** — think a whole new topic is missing? Propose it.

---

## File Structure

```
Commands Guide/
├── README.md                   ← Homepage (do not remove any links)
├── CONTRIBUTING.md             ← This file
└── commands/
    ├── docker.md
    ├── linux.md
    ├── git.md
    ├── kubernetes.md
    ├── npm-yarn.md
    └── windows-powershell.md
```

Each category lives in its own Markdown file under `commands/`.

---

## Command Format

Every command entry should follow this structure:

### Short description

```bash
command --flag argument       # Inline comment explaining what it does
command --other-flag          # Another variant
```

> Optional tip or warning about the command.

---

### Rules

1. **Use fenced code blocks** with the correct language tag (`bash`, `powershell`, `yaml`, etc.).
2. **Add inline comments** with `#` to explain what each command does.
3. **Mark destructive commands** with a blockquote warning:
   > ⚠️ This action is irreversible. Double-check before running.
4. **Group related commands** under a `###` heading.
5. **Use comparison tables** (npm vs Yarn, CMD vs PowerShell) where the parallel helps.

---

### Example entry

````markdown
### Pull remote changes

```bash
git pull                      # Fetch + merge from tracked branch
git pull origin main          # Explicitly pull from origin/main
git pull --rebase             # Rebase instead of merge
```

> ⚠️ `--rebase` rewrites commit history. Avoid on shared branches.
````

---

## Adding a New Category

1. Create a new file: `commands/<category-name>.md`
   - Use lowercase with hyphens: `ci-cd.md`, `aws-cli.md`
2. Use this file header:

```markdown
<div align="center">

# 🔧 Category Name

</div>

[← Back to Home](../README.md)

---

## Table of Contents
...
```

3. Add a navigation footer at the bottom:

```markdown
[← Previous Category](previous.md) | [Back to Home](../README.md)
```

4. **Update `README.md`**:
   - Add a row to the category table with the emoji, name, description, and link.
   - Add an entry to the Table of Contents.

---

## Submitting a Pull Request

1. **Fork** this repository.
2. **Create a branch** for your change:
   ```bash
   git checkout -b add-aws-commands
   ```
3. **Make your changes** following the format guidelines above.
4. **Commit** with a clear message:
   ```bash
   git commit -m "Add AWS CLI commands file"
   git commit -m "Fix: incorrect flag in docker run example"
   ```
5. **Push** and open a Pull Request against `main`.
6. Describe what you added/changed and why in the PR description.

---

## Style Guidelines

| Rule | Example |
|------|---------|
| Section headers use `##` and `###` only | `## Branching`, `### Create a branch` |
| Commands in backticks inline | "Run `git status` to see changes" |
| Code blocks use language tags | ` ```bash `, ` ```powershell ` |
| Emoji in section headers are welcome | `## 🌿 Branching` |
| Keep lines under 120 characters | — |
| One blank line between sections | — |

---

Thank you for making this resource better for everyone!
