# GitHub Copilot Coding Agent Structure Guide

## Overview

This document defines the comprehensive structure and guidelines that the coding agent should follow when working with Phoenix applications using Elixir. All files, documentation, and code should be created in **English** following these established patterns and conventions.

## Directory Structure

The `.github` system provides a complete organizational framework for AI-driven development:

```
.github/
├── agents/                    # Specialized agent definitions
│   ├── context-fetcher.md     # Intelligent context retrieval agent
│   ├── file-creator.md        # File and template creation agent
│   ├── git-workflow.md        # Git workflow management agent
│   └── test-runner.md         # Test execution and validation agent
├── commands/                  # High-level command definitions
│   ├── analyze-product.md     # Product analysis command
│   ├── create-spec.md         # Specification creation command
│   ├── execute-tasks.md       # Task execution command
│   └── plan-product.md        # Product planning command
├── instructions/              # Detailed operational instructions
│   ├── core/                  # Core instruction set
│   │   ├── analyze-product.md # Product analysis workflow
│   │   ├── create-spec.md     # Specification creation workflow
│   │   ├── execute-task.md    # Single task execution
│   │   ├── execute-tasks.md   # Batch task execution
│   │   └── plan-product.md    # Product planning workflow
│   └── meta/                  # Meta-instructions
│       └── pre-flight.md      # Pre-execution validation checks
├── libraries_docs/            # Third-party library documentation
├── phoenix_guide/             # Phoenix framework-specific guides
│   ├── controllers.md         # Controller patterns and best practices
│   ├── security.md            # Security implementation guidelines
│   ├── data_modelling/        # Data modeling patterns
│   │   ├── contexts.md        # Phoenix context patterns
│   │   ├── schemas.md         # Ecto schema patterns
│   │   └── migrations.md      # Database migration patterns
│   ├── deployment/            # Deployment strategies
│   │   ├── docker.md          # Docker configuration
│   │   ├── releases.md        # Elixir releases
│   │   └── monitoring.md      # Application monitoring
│   └── howto/                 # Specific implementation guides
│       ├── authentication.md  # User authentication
│       ├── real-time.md       # Phoenix LiveView/Channels
│       └── testing.md         # Testing strategies
└── standards/                 # Development standards and conventions
    ├── best-practices.md      # General development best practices
    ├── code-style.md          # Overall code style guidelines
    ├── tech-stack.md          # Default technology stack
    └── code-style/            # Language-specific style guides
        ├── css-style.md       # CSS and daisyUI conventions
        ├── html-style.md      # HTML template conventions
        └── javascript-style.md # JavaScript/TypeScript conventions
```
