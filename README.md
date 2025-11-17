<p align="center">
  <img src="images/gsk-logo.svg" width="300">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/status-active-brightgreen">
  <img src="https://img.shields.io/badge/shell-bash%20%2F%20zsh-blue">
  <img src="https://img.shields.io/badge/source-private-lightgrey">
</p>

# GSK (Git Script Kit)

* * *

## 📌 Overview

The **GSK (Git Script Kit)** is a corporate framework for Git and DevOps automation.
Developed entirely in Shell Script, GSK standardizes branching workflows (GSK Flow), encapsulates complex Git operations (submodules, LFS, merge, rebase, etc.), implements robust validations, structured logging, Azure DevOps REST API integration, and provides support for CI/CD pipelines.
The framework reduces operational errors, simplifies daily routines, and increases efficiency for development and operations teams.

GSK is designed with the following principles:

- **Single Responsibility Principle (SRP)**
- **Modularity**
- **Function reuse**
- **Workflow standardization**
- **Clear and extensible architecture**

* * *

## ❓ Why This Project Exists

GSK was created to address real challenges in large-scale development environments, where Git usage varies significantly among teams and operational mistakes can lead to rework, broken pipelines, and inconsistencies.

Before GSK, common issues included:

- Branching inconsistencies.
- Dangerous or repetitive manual Git operations.
- Merge conflicts caused by improper workflows.
- Lack of uniformity between Windows, WSL, and Linux users.
- No integration between Git actions and Azure DevOps.
- Recurring human errors affecting SCM and DevOps processes.

The framework was built to:

- Enforce a reliable and standardized Git workflow.
- Automate complex or error-prone Git routines.
- Provide safety layers through validation and structured logs.
- Unify tooling across different shells and operating systems.
- Integrate Git processes with Azure DevOps APIs.
- Support DevOps, SCM and development teams with predictable workflows.

GSK has evolved continuously since 2011 and remains an essential tool for efficient and consistent delivery.

* * *

## 🔑 Key Features

- Unified CLI for Git automation.
- Standardized branching workflow (**GSK Flow**).
- Automation of complex Git tasks (merge, rebase, LFS, submodules).
- Azure DevOps REST API integration.
- Cross-shell compatibility (**Bash** and **Zsh**).
- Multi-platform support (**Windows**, **WSL** and **Linux**).
- Modular architecture with reusable internal functions.
- Built-in help system with parameter validation.
- Structured logging for troubleshooting and auditing.
- CI/CD integration for Jenkins and Azure DevOps pipelines.

* * *

## 🧩 Script Architecture

Every script follows a structured design with four core methods:

- `gskConstants` - Loads configurations and internal variables.
- `gskHelp` - Displays documentation, parameters, and usage examples.
- `gskValidate` - Performs input validation and pre-condition checks.
- `gskExec` - Contains the main execution logic.

The internal project structure is organized into folders based on purpose and audience:

    dev/
    functions/
    system/
    deprecated/
    scm/
    manager/

* * *

## 🚀 Key Benefits

### ✔ Simplified Git usage
Provides intuitive commands with syntax similar to native Git operations.

### ✔ Error prevention
Validation layers, structured logs, informative messages, and automated handling reduce operational mistakes.

### ✔ Reuse and maintainability
Each script follows SRP, avoiding duplicated logic and enabling cleaner evolution.

### ✔ Corporate integrations
Includes native integration with the **Azure DevOps REST API**, enabling tasks such as:
- branch management;
- work item interactions;
- group and permission queries;
- automated pipeline routines.

### ✔ Cross-platform compatibility
Works consistently across:
- **Bash** and **Zsh**
- **Windows**
- **WSL**
- **Linux**

### ✔ Standardized workflow
Native support for **GSK Flow**, a Git workflow model inspired by Git Flow and GitLab Flow, developed exclusively for the framework.

### ✔ CI/CD process automation
Scripts can be used in Jenkins or Azure DevOps pipelines, reducing manual work and ensuring process consistency.

* * *

## 🔧 Additional Features

- Intelligent auto-complete
- Integrated help for all commands
- Windows context-menu integration
- Parameter validation, standardized color scheme, and structured execution logs

* * *

## ⚠ Restrictions and Best Practices

To ensure integrity and compatibility across versions:

- **Do NOT move scripts between directories**
- **Do NOT edit scripts directly**
- **Do NOT rename internal scripts**

> 🔶 _Any manual modification will be automatically discarded during the update process_.

For safe customization:

- Define custom aliases  
- Add external user-defined scripts  

* * *

## 👤 Author

**Created and maintained by:**  
**Alex Ferreira de Almeida**  
Software Engineer / DevOps & SCM Specialist  

* * *

## 📝 Disclaimer

This repository contains **public documentation only**.  
The actual source code of the GSK is private and cannot be published due to internal processes, proprietary integrations, and corporate security policies.

This README exists solely to document the project’s existence, architecture, design principles, and authorship.

* * *

## 📅 Project Status

**Active - 2011 to Present**  
Corporate internal tool currently used in production environments.
