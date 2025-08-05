# GitHub Copilot Custom Instructions - Implementation Guide

## Overview

This document explains how the Phoenix Rules repository implements GitHub Copilot Custom Instructions following the official best practices from GitHub's documentation.

## Implementation Strategy

### 1. Main Instructions File

The primary `copilot-instructions.md` file provides:
- **Project Overview**: Clear description of the Phoenix Rules framework
- **Folder Structure**: Organized documentation of the .github directory
- **Libraries and Frameworks**: Complete technology stack specification
- **Coding Standards**: Comprehensive development guidelines
- **UI Guidelines**: Design system and component standards

### 2. Scoped Instruction Files

Following GitHub's recommendation for `.instructions.md` files with `applyTo` frontmatter:

- **`phoenix-development.instructions.md`**: Core Phoenix patterns for `lib/**/*.ex`
- **`phoenix-web.instructions.md`**: Web layer patterns for `lib/**/*_web/**/*.ex`
- **`testing.instructions.md`**: Testing guidelines for `test/**/*.exs`
- **`frontend.instructions.md`**: Asset development for `assets/**/*.{css,js,ts}`
- **`migrations.instructions.md`**: Database patterns for `priv/repo/migrations/*.exs`
- **`configuration.instructions.md`**: Config patterns for `config/*.exs`

### 3. Agent System Integration

The specialized agent system (`context-fetcher`, `file-creator`, `test-runner`, `git-workflow`) is documented to work seamlessly with GitHub Copilot's workflow understanding.

## Benefits of This Structure

1. **Targeted Guidance**: File-specific instructions provide relevant context only when needed
2. **Reduced Token Usage**: Scoped instructions avoid sending irrelevant information to the model
3. **Maintainability**: Separate files are easier to update and manage
4. **Scalability**: New file patterns can be added with dedicated instruction files

## Best Practices Applied

Following GitHub's recommendations, the instructions:
- Are short and self-contained
- Provide relevant project context
- Include folder structure information
- Specify coding standards and conventions
- List tools, libraries, and frameworks with versions
- Avoid overly specific formatting requirements
- Focus on broadly applicable guidance

## Usage in VS Code

1. Enable custom instructions in VS Code settings
2. The `.github/copilot-instructions.md` file is automatically used
3. Scoped `.instructions.md` files apply to specific file patterns
4. References appear in Copilot Chat responses for verification

## Maintenance Guidelines

- Keep instructions concise and focused
- Update version numbers when making significant changes
- Test instructions with real development scenarios
- Review and update libraries/frameworks versions regularly
- Ensure consistency between main and scoped instruction files

## Compatibility

This implementation supports:
- GitHub Copilot Chat in VS Code
- GitHub Copilot coding agent
- GitHub Copilot Chat on github.com
- GitHub Copilot code review features

The structure is designed to be forward-compatible with future GitHub Copilot features and improvements.
