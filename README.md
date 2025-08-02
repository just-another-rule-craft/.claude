# Phoenix Rules - Rules System for Development with Claude AI

## Overview

This project contains a complete system of rules and templates for developing Phoenix applications with Elixir, specifically designed to work with Claude AI. The `.claude` system provides an organizational structure that allows Claude to create, plan, and execute projects consistently and efficiently.

## System Architecture

### Directory Structure

```
.claude/
├── agents/                    # Specialized agents
│   ├── context-fetcher.md     # Intelligent context retrieval
│   ├── file-creator.md        # File and template creation
│   ├── git-workflow.md        # Git workflow management
│   └── test-runner.md         # Test execution
├── commands/                  # Main commands
│   ├── analyze-product.md     # Analysis of existing products
│   ├── create-spec.md         # Specification creation
│   ├── execute-tasks.md       # Task execution
│   └── plan-product.md        # Product planning
├── instructions/              # Detailed instructions
│   ├── core/                  # Main instructions
│   │   ├── analyze-product.md
│   │   ├── create-spec.md
│   │   ├── execute-task.md
│   │   ├── execute-tasks.md
│   │   └── plan-product.md
│   └── meta/                  # Meta-instructions
│       └── pre-flight.md      # Pre-flight checks
├── phoenix_guide/             # Phoenix-specific guides
│   ├── controllers.md         # Controller patterns
│   ├── security.md           # Security practices
│   ├── data_modelling/       # Data modeling
│   │   ├── contexts.md       # Context handling
│   │   └── ...
│   ├── deployment/           # Deployment
│   └── howto/               # Specific guides
└── standards/                # Standards and best practices
    ├── best-practices.md     # General best practices
    ├── code-style.md         # Code style guide
    ├── tech-stack.md         # Default tech stack
    └── code-style/           # Specific styles
        ├── css-style.md      # CSS and daisyUI styles
        ├── html-style.md     # HTML styles
        └── javascript-style.md
```

## Main Components

### 1. Specialized Agent System

The system uses specialized agents that work together:

#### Context Fetcher Agent
- **Purpose**: Intelligent retrieval of documentation and context
- **Capabilities**:
  - Verifies if information is already in context
  - Extracts specific sections from files
  - Uses grep searches to find relevant information
  - Avoids content duplication

#### File Creator Agent
- **Purpose**: Creation of files and directories with consistent templates
- **Capabilities**:
  - Template handling for different file types
  - Directory structure creation
  - Application of naming conventions
  - Batch operations for multiple files

#### Git Workflow Agent
- **Purpose**: Complete Git workflow management
- **Capabilities**:
  - Branch handling with naming conventions
  - Creation of descriptive commits
  - Pull request management
  - Status verification before operations

#### Test Runner Agent
- **Purpose**: Execution and verification of test suites
- **Capabilities**:
  - Complete test execution
  - Failure detection and reporting
  - Integration with development workflows

### 2. High-Level Commands

#### Plan Product
Generates complete documentation for new products:
- **Generated files**:
  - `mission.md`: Product vision and purpose
  - `mission-lite.md`: Condensed version for AI
  - `tech-stack.md`: Technical architecture
  - `roadmap.md`: Development phases
  - `decisions.md`: Decision log

#### Create Spec
Creates detailed specifications for features:
- **8-step process**:
  1. Specification initiation
  2. Context gathering
  3. Requirements clarification
  4. Date determination
  5. Specification folder creation
  6. `spec.md` generation
  7. `spec-lite.md` creation
  8. Technical specification

#### Execute Tasks
Task execution system with complete flow:
- **10-step flow**:
  1. Task assignment
  2. Context analysis
  3. Development server verification
  4. Git branch management
  5. Task execution loop
  6. Test suite verification
  7. Complete Git workflow
  8. Roadmap progress verification
  9. Completion notification
  10. Completeness summary

### 3. Standards and Best Practices

#### Default Tech Stack
- **Framework**: Phoenix 1.8.0-rc.4
- **Language**: Elixir 1.18.1
- **Database**: PostgreSQL 17+
- **ORM**: ecto_sql 3.10
- **CSS**: TailwindCSS 4.0+
- **UI Components**: daisyUI 5
- **Icons**: heroicons
- **CI/CD**: GitHub Actions

#### Development Principles
- **Simplicity**: Implement in the fewest lines possible
- **Readability**: Prioritize clarity over micro-optimizations
- **DRY**: Don't repeat code
- **File structure**: Single responsibility per file

## Main Workflows

### 1. Create a New Product

```bash
# Use the plan-product command
# Generates all base product documentation
```

**Generated files**:
- `.claude/product/mission.md`
- `.claude/product/mission-lite.md`
- `.claude/product/tech-stack.md`
- `.claude/product/roadmap.md`
- `.claude/product/decisions.md`

### 2. Create a New Feature

```bash
# Use the create-spec command
# Generates complete specification with tasks
```

**Generated structure**:
```
.claude/specs/YYYY-MM-DD-feature-name/
├── spec.md              # Complete specification
├── spec-lite.md         # AI summary
├── tasks.md             # Task breakdown
└── sub-specs/
    ├── technical-spec.md
    ├── database-schema.md
    ├── api-spec.md
    └── tests.md
```

### 3. Execute Development Tasks

```bash
# Use the execute-tasks command
# Executes the entire development flow
```

**Automated process**:
1. Task selection
2. Environment setup
3. Git management
4. Implementation
5. Testing
6. Pull Request

## Phoenix Integration

### Contextual Guides
The system includes complete Phoenix guides that are dynamically loaded based on context:

- **Controllers**: Patterns and best practices
- **Security**: Authentication and authorization
- **Contexts**: Data modeling and boundaries
- **Deployment**: Production configuration

### Code Styles
- **CSS**: Complete integration with daisyUI 5 and TailwindCSS 4
- **HTML**: Semantic structure and accessibility
- **JavaScript**: Modern patterns and best practices

## Advanced Features

### 1. Conditional Context Loading
The system automatically verifies what information is already in context to avoid duplication and optimize performance.

### 2. Intelligent Templates
All templates include metadata, versioning, and consistent structure.

### 3. Automatic Validation
Built-in verifications at each process step to ensure quality.

### 4. Complete Git Integration
Automatic management of branches, commits, and pull requests following best practices.

## Practical Usage

### For Developers
1. **Initial setup**: Execute `plan-product` for new projects
2. **Feature development**: Use `create-spec` followed by `execute-tasks`
3. **Existing code analysis**: Use `analyze-product`

### For Teams
- **Consistent standards**: All projects follow the same conventions
- **Automatic documentation**: Automatic generation of technical documentation
- **Unified workflow**: Standard development and deployment process

## Extensibility

The system is designed to be extensible:
- **New agents**: Add specialized agents for specific functionalities
- **Custom templates**: Modify or add new templates
- **Specific standards**: Adapt standards for different project types

## Key Benefits

1. **Consistency**: Uniform structure across all projects
2. **Efficiency**: Automation of repetitive tasks
3. **Quality**: Integrated standards and best practices
4. **Scalability**: Modular and extensible system
5. **Collaboration**: Standardized documentation and processes

This system represents a complete solution for professional Phoenix application development with AI assistance, providing structure, consistency, and efficiency throughout the entire development process.

## Inspiration and Credits

This project was inspired by the excellent work done in [agent-os](https://github.com/buildermethods/agent-os/tree/main) by the creator at Builder Methods. Their innovative approach to AI-driven development workflows provided the foundational concepts that we've adapted and extended for Phoenix development.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### MIT License Summary

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software.

**This is completely open source software - you are free to use, modify, and distribute it as you see fit.**