# GitHub Copilot Custom Instructions for Phoenix Rules

## Project Overview

This project is a comprehensive Phoenix application development framework that provides structured guidelines, patterns, and automated workflows for building Elixir/Phoenix applications. It serves as a foundation for creating scalable, maintainable web applications following Phoenix best practices.

**Purpose**: Provide AI-driven development assistance with specialized agents and workflows for Phoenix application development.

**Goals**:
- Standardize Phoenix development patterns and conventions
- Automate common development workflows through specialized agents
- Ensure consistent code quality and architectural decisions
- Facilitate rapid prototyping and feature development

## Folder Structure

The `.github` directory contains all AI-driven development infrastructure:

```
- `/agents/`: Specialized AI agents for specific development tasks
- `/commands/`: High-level command definitions for complex workflows
- `/instructions/`: Detailed operational instructions and workflows
  - `/core/`: Core instruction set for primary development tasks
  - `/meta/`: Meta-instructions and pre-flight checks
- `/libraries_docs/`: Third-party library documentation and integration guides
- `/phoenix_guide/`: Phoenix framework-specific patterns and best practices
  - `/data_modelling/`: Ecto schemas, contexts, and database patterns
  - `/deployment/`: Production deployment strategies
  - `/howto/`: Specific implementation guides
  - `/authn_authz/`: Authentication and authorization patterns
  - `/real_time/`: Channels and LiveView patterns
  - `/testing/`: Testing strategies and patterns
- `/standards/`: Development standards and coding conventions
  - `/code-style/`: Language-specific style guides

## Libraries and Frameworks

- **Phoenix Framework**: 1.8.0-rc.4 for web application structure
- **Elixir**: 1.18.1 for backend development
- **Ecto**: Database wrapper and query generator
- **PostgreSQL**: Primary database system (17+)
- **TailwindCSS**: 4.0+ for styling and responsive design
- **daisyUI**: 5.0+ for UI components and design system
- **esbuild**: Asset bundling and JavaScript compilation
- **ExUnit**: Testing framework for Elixir applications

## Coding Standards

**General Principles**:
- Write clear, readable code with descriptive variable and function names
- Follow Elixir naming conventions (snake_case for functions, PascalCase for modules)
- Implement comprehensive error handling and validation
- Use pattern matching and guards effectively
- Write documentation for all public functions

**Phoenix-Specific**:
- Follow Phoenix context patterns for business logic organization
- Use LiveView for interactive UI components
- Implement proper security measures (CSRF protection, input validation)
- Follow RESTful conventions for controller actions
- Use Phoenix generators for consistency

**Testing**:
- Write tests for all business logic functions
- Use descriptive test names that explain the expected behavior
- Follow the Arrange-Act-Assert pattern
- Test both success and failure scenarios
- Use factories for test data creation

## Project-Specific Guidelines

**Agent System**:
- Use specialized agents (`context-fetcher`, `file-creator`, `git-workflow`, `test-runner`) for specific tasks
- Follow the defined workflow patterns in `/instructions/core/`
- Always validate prerequisites before executing complex workflows

**File Organization**:
- Place Phoenix-specific patterns in `/phoenix_guide/`
- Store coding standards in `/standards/`
- Document decisions and technical choices in appropriate locations
- Maintain consistent file naming and structure

**Development Workflow**:
- Follow Test-Driven Development (TDD) practices
- Use conventional commit messages
- Implement features through the defined spec → task → implementation cycle
- Ensure all changes include appropriate tests and documentation

## UI Guidelines

- Use daisyUI components for consistent design language
- Implement responsive design using TailwindCSS utilities
- Follow accessibility best practices (ARIA labels, semantic HTML)
- Maintain consistent spacing and typography throughout the application
- Use CSS variables for theme customization

## Specialized Instructions

This repository includes file-specific instruction files for targeted guidance:

- **Phoenix Development** (`phoenix-development.instructions.md`): Core Phoenix patterns for `lib/**/*.ex` files
- **Phoenix Web Layer** (`phoenix-web.instructions.md`): Web-specific patterns for controllers and views
- **Testing** (`testing.instructions.md`): Comprehensive testing guidelines for `test/**/*.exs` files
- **Frontend Assets** (`frontend.instructions.md`): TailwindCSS and JavaScript best practices
- **Database Migrations** (`migrations.instructions.md`): Migration patterns and database design
- **Configuration** (`configuration.instructions.md`): Environment and application configuration

## Agent Workflow Integration

When working with this repository, leverage the specialized agent system:

1. **Use `context-fetcher`** for retrieving relevant documentation and patterns
2. **Use `file-creator`** for generating files with proper templates and structure
3. **Use `test-runner`** for executing and validating test suites
4. **Use `git-workflow`** for managing version control and deployment processes

Follow the established workflows in `/instructions/core/` for complex development tasks.
```
