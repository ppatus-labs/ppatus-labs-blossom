# 🌸 Blossom Git Standards

This repository follows the **Conventional Commits** specification. This ensures a clean, readable history and enables **Automated Semantic Versioning** via GitHub Actions.

</br>

## 🌸 Why Blossom?

Inspired by the philosophy of **Team Cherry**, Blossom is built on the idea that even the smallest "Lab" project deserves world-class polish.

In an industry often driven by extraction, Team Cherry stands as a reminder that **great things can be built simply for the joy of it.** They created a masterpiece out of a passion project, choosing to prioritize accessibility and depth over profit—offering a world far more expansive than its price tag would suggest, simply because they were "having too much fun" to stop adding to it.

**Blossom** carries this DNA. It’s a framework for those who believe that:

* **The "Lab" stage is sacred:** Experimentation should be fueled by curiosity, not quotas.
* **Quality is a gift to the user:** Even a small utility should feel solid, intentional, and respectful of the person using it.
* **Integrity scales:** By seeding projects with high-fidelity standards from day one, we ensure they have the structural integrity to become something enduring.

_Disclaimer: this short introduction was made using an LLM so it may get a bit cocky, but you get the point._

</br>

# 🚀 Quick Start & Onboarding

Follow these steps to initialize your local environment and sync with the Blossom automation.

### 1. GitHub Repository Setup
Before pushing code, grant the workflow write access (Required once per repo):
- [ ] Go to **Settings > Actions > General**.
- [ ] Set **Workflow permissions** to **Read and write permissions**.
- [ ] Check **Allow GitHub Actions to create and approve pull requests**.

### 2. Local Environment Setup
Ensure you have `pre-commit` installed:
```bash
# macOS
brew install pre-commit

# Linux/Windows
pip install pre-commit
```

Once installed, link the configuration to your local Git hooks:
```bash
pre-commit install --install-hooks
```

Due to our `default_install_hook_types` configuration in `.pre-commit-config.yaml`, this command automatically activates two distinct Git hooks:

* **`pre-commit`:**
    This hook runs **before** the commit message editor opens. Used typically to scan your **staged files** to ensure code hygiene (e.g., removing trailing whitespace, fixing end-of-file formatting, or checking for syntax errors) before a snapshot is attempted.

* **`commit-msg`:**
    This hook runs **after** you write your message but **before** the commit is finalized. It strictly validates the **text structure** of your message to ensure it adheres to Conventional Commits (e.g., `feat: ...` or `fix: ...`), which is required for the automated versioning system to function.


### 🔍 Why use an External Repository for Hooks?

You might notice that the `.pre-commit-config.yaml` points to an external repo (`https://github.com/compilerla/...`). Here is why:

* **Modular Logic:** `pre-commit` acts as a package manager for Git hooks. Instead of writing complex scripts to parse regex and validate commit strings from scratch, we pull a **versioned, community-tested tool**.
* **Isolation:** The hook logic is kept separate from your project's source code. When you run `pre-commit install`, the tool clones the logic into a central cache on your machine, keeping your repository clean.
* **Stability (The `rev` tag):** We use a specific version tag (e.g., `rev: v3.1.0`) instead of `main`. This ensures that your workflow never breaks due to unexpected updates from the maintainers.



#### Updating Hooks
To safely update the hook logic to the latest version, run:
```bash
pre-commit autoupdate
```
</br>

## 📝 Commit Message Convention

All commit messages must follow this structure:  
`<type>(scope): <short description in lowercase>`



* **Type:** The category of the change (see table below).
* **Scope (Optional):** A noun describing the specific part of the project affected (e.g., `ui`, `engine`, `assets`). It must be enclosed in parentheses. They must be defined previously in `.pre-commit-config.yaml`.
* **Description:** A concise summary of the change in lowercase.

### Examples
- `feat(ui): add cherry-themed loading spinner`
- `fix(core): resolve memory leak in physics loop`
- `docs(readme): add installation instructions`

### Allowed Types

| Type | Description | Release Impact |
| :--- | :--- | :--- |
| **feat** | A new feature | **Minor** (v1.X.0) |
| **fix** | A bug fix | **Patch** (v1.0.X) |
| **docs** | Documentation only changes | None |
| **style** | Formatting, missing semi-colons, etc. | None |
| **refactor** | Code change that neither fixes a bug nor adds a feature | None |
| **perf** | A code change that improves performance | **Patch** |
| **test** | Adding/correcting tests | None |
| **build** | Changes to build system or dependencies | None |
| **ci** | Changes to CI configuration | None |
| **chore** | General maintenance tasks | None |

> **Note on Scopes:** In this template, scopes are currently "open" (you can type anything). To restrict them to a specific list, edit the `--allowed-scopes` argument in `.pre-commit-config.yaml`.

### Breaking Changes
To signal a **Major** release (vX.0.0), add an `!` after the type/scope or include `BREAKING CHANGE:` in the footer:
* `feat(api)!: remove version 1 endpoints`
* `fix!: rewrite core rendering logic`

### Requirements
* **GitHub Token:** The semantic versioning workflow uses the built-in `GITHUB_TOKEN`. Ensure your repository settings allow GitHub Actions to create Tags and Releases (**Settings > Actions > General > Workflow permissions** must be set to "Read and write permissions").

</br>

## 🙈 Ignored Files

This repository includes a **Universal `.gitignore`** to keep the codebase clean. It is configured to ignore:
* **System files:** OS-generated clutter (like `.DS_Store` or `Thumbs.db`).
* **IDE settings:** Personal configurations from VS Code, IntelliJ, etc.
* **Sensitive data:** Environment variables (`.env`) and private keys to prevent accidental leaks.
* **Cache:** Temporary files generated by `pre-commit` and other build tools.
* **Node.js:** Automatically ignores the heavy `node_modules` folder and common build outputs (`dist/`, `out/`).
* **Python:** Filters out compiled bytecode (`__pycache__`) and local virtual environments (`.venv`), ensuring you don't accidentally commit thousands of library files.