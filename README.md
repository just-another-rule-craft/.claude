# Phoenix Rules - AI-Powered Development System for Phoenix Applications

## Overview

This project contains a comprehensive dual-system architecture for developing Phoenix applications with Elixir, designed to work seamlessly with both **Claude AI** and **GitHub Copilot**. The system combines the power of two AI assistants through complementary directory structures that enable consistent, efficient, and high-quality Phoenix development.

## 🏗️ Dual System Architecture

### `.claude/` - Claude AI Development System
**Purpose**: Complete project lifecycle management with Claude AI

### `.github/` - GitHub Copilot Integration
**Purpose**: Code quality, patterns, and collaborative development with GitHub Copilot

## 📁 Directory Structure Overview

### `.claude/` - Claude AI System
```
.claude/
├── agents/                    # Specialized AI agents
│   ├── context-fetcher.md     # Intelligent context retrieval
│   ├── file-creator.md        # File and template creation
│   ├── git-workflow.md        # Git workflow management
│   ├── refactor-agent.md      # Code refactoring specialist
│   └── test-runner.md         # Test execution and verification
├── commands/                  # High-level development commands
│   ├── analyze-product.md     # Product analysis and assessment
│   ├── create-spec.md         # Feature specification creation
│   ├── execute-tasks.md       # Task execution workflows
│   ├── plan-product.md        # Product planning and architecture
│   ├── implement-phoenix-feature.md # Feature implementation
│   ├── debug-phoenix-issue.md # Debugging workflows
│   ├── api-dev.md            # API development
│   ├── deploy-monitor.md     # Deployment and monitoring
│   └── security-audit.md     # Security auditing
├── instructions/              # Detailed process instructions
│   ├── core/                  # Core development instructions
│   └── meta/                  # Meta-process instructions
├── phoenix_guide/             # Phoenix framework expertise
│   ├── controllers.md         # Controller patterns
│   ├── live_view.md          # LiveView development
│   ├── security.md           # Security best practices
│   ├── data_modelling/       # Data modeling and contexts
│   ├── deployment/           # Production deployment
│   └── testing/              # Testing strategies
├── standards/                 # Development standards
│   ├── best-practices.md     # General best practices
│   ├── code-style.md         # Code style guidelines
│   ├── tech-stack.md         # Technology specifications
│   └── claude-code-optimization.md # Claude Code integration
└── CLAUDE.md                 # Main Claude configuration
```

### `.github/` - GitHub Copilot System
```
.github/
├── copilot-instructions.md   # Main GitHub Copilot configuration
├── coding-guidelines/        # Code quality guidelines
│   ├── phoenix-patterns.md  # Phoenix-specific patterns
│   ├── elixir-style.md      # Elixir coding standards
│   ├── testing-standards.md # Testing approaches
│   ├── security-practices.md# Security guidelines
│   └── performance-optimization.md # Performance patterns
├── ISSUE_TEMPLATE/          # GitHub issue templates
│   ├── bug_report.yml       # Bug reporting template
│   └── feature_request.yml  # Feature request template
├── pull_request_template.md # PR template for code review
└── dependabot.yml          # Dependency management
```

## 🎯 System Components

### 1. Claude AI System (`.claude/`)

#### Specialized Agent Ecosystem
The Claude system uses specialized agents that work together for complete project management:

**Context Fetcher Agent**
- **Purpose**: Intelligent retrieval of documentation and context
- **Capabilities**:
  - Verifies if information is already in context
  - Extracts specific sections from files
  - Uses semantic searches to find relevant information
  - Avoids content duplication

**File Creator Agent**
- **Purpose**: Creation of files and directories with consistent templates
- **Capabilities**:
  - Template handling for different file types
  - Directory structure creation
  - Application of naming conventions
  - Batch operations for multiple files

**Git Workflow Agent**
**Git Workflow Agent**
- **Purpose**: Complete Git workflow management
- **Capabilities**:
  - Branch handling with naming conventions
  - Creation of descriptive commits
  - Pull request management
  - Status verification before operations

**Test Runner Agent**
- **Purpose**: Execution and verification of test suites
- **Capabilities**:
  - Complete test execution
  - Failure detection and reporting
  - Integration with development workflows

**Refactor Agent**
- **Purpose**: Code refactoring and optimization
- **Capabilities**:
  - Pattern recognition and improvements
  - Code structure optimization
  - Legacy code modernization

#### High-Level Development Commands

**Plan Product** (`plan-product`)
- Generates complete documentation for new products
- Creates mission, technical architecture, and roadmap
- Establishes project foundation and decision logs

**Create Spec** (`create-spec`)
- Creates detailed specifications for features
- 8-step specification process with complete documentation
- Generates technical specs and task breakdowns

**Execute Tasks** (`execute-tasks`)
- Complete task execution workflow
- 10-step automated development process
- Integrates testing, Git workflow, and deployment

**Implement Phoenix Feature** (`implement-phoenix-feature`)
- Feature-focused development workflow
- Phoenix-specific implementation patterns
- Automated testing and validation

### 2. GitHub Copilot System (`.github/`)

#### Code Quality and Collaboration

**Copilot Instructions**
- **Purpose**: Main configuration for GitHub Copilot integration
- **Features**:
  - Phoenix-specific code patterns
  - LiveView development guidelines
  - Context-aware suggestions
  - Performance optimization hints

**Coding Guidelines**
- **Phoenix Patterns**: Framework-specific best practices
- **Elixir Style**: Language conventions and idioms
- **Testing Standards**: Comprehensive testing approaches
- **Security Practices**: Security-first development
- **Performance Optimization**: Performance-conscious coding

**GitHub Integration**
- **Issue Templates**: Structured bug reports and feature requests
- **Pull Request Template**: Comprehensive code review checklist
- **Dependabot**: Automated dependency management
- **No CI/CD**: Focused on development patterns, not deployment

## 🔧 Technology Stack

#### Framework & Language
- **Framework**: Phoenix 1.8.0-rc.4
- **Language**: Elixir 1.18.1
- **Database**: PostgreSQL 17+
- **ORM**: ecto_sql 3.10

#### Frontend & Styling
- **CSS**: TailwindCSS 4.0+
- **UI Components**: daisyUI 5
- **Icons**: heroicons

#### Development Principles
- **Simplicity**: Implement in the fewest lines possible
- **Readability**: Prioritize clarity over micro-optimizations
- **DRY**: Don't repeat code
- **File structure**: Single responsibility per file
- **AI-First**: Designed for AI-assisted development

## 🚀 Main Workflows

### 1. Project Initialization (Claude AI)

**Command**: `plan-product`
```bash
# Creates complete project foundation
# Generates mission, architecture, and roadmap
```

**Generated structure**:
```
.claude/product/
├── mission.md           # Product vision and goals
├── mission-lite.md      # AI-optimized summary
├── tech-stack.md        # Technical architecture
├── roadmap.md          # Development phases
└── decisions.md        # Decision log
```

### 2. Feature Development (Claude AI)

**Command**: `create-spec` + `execute-tasks`
```bash
# Step 1: Create specification
# Step 2: Execute implementation
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

### 3. Code Quality (GitHub Copilot)

**Automatic Integration**:
- Real-time code suggestions based on Phoenix patterns
- Security-first recommendations
- Performance optimization hints
- Testing pattern suggestions

### 4. Collaborative Development

**GitHub Integration**:
- Structured issue reporting
- Comprehensive PR reviews
- Automated dependency updates
- Consistent code patterns across team

## 🧠 AI Integration Features

### Claude AI Capabilities
- **Project Planning**: Complete product architecture and roadmap generation
- **Specification Creation**: Detailed feature specifications with task breakdowns
- **Implementation**: Full development workflows with automated testing
- **Context Management**: Intelligent information retrieval and context awareness
- **Git Integration**: Automated branch management, commits, and pull requests

### GitHub Copilot Capabilities
- **Code Completion**: Phoenix and Elixir-aware code suggestions
- **Pattern Recognition**: Framework-specific patterns and best practices
- **Security Awareness**: Security-first code recommendations
- **Performance Optimization**: Performance-conscious code suggestions
- **Testing Integration**: Test-driven development patterns

### Dual AI Workflow
1. **Planning Phase**: Claude AI creates specifications and project structure
2. **Implementation Phase**: GitHub Copilot assists with code completion and patterns
3. **Review Phase**: Both systems ensure quality and consistency
4. **Deployment Phase**: Claude AI manages the complete workflow

## 📋 Practical Usage

### For Individual Developers
1. **Project Setup**: Use Claude AI's `plan-product` for new projects
2. **Feature Development**: `create-spec` + `execute-tasks` with Claude AI
3. **Code Implementation**: GitHub Copilot for real-time assistance
4. **Code Review**: Integrated quality checks and patterns

### For Development Teams
- **Consistent Standards**: Unified coding guidelines across both AI systems
- **Automated Documentation**: Automatic generation of technical specifications
- **Standardized Workflow**: Consistent development and review processes
- **Knowledge Sharing**: Centralized patterns and best practices

### Integration with Development Tools
- **VS Code**: Native GitHub Copilot integration
- **Claude Code**: Direct integration with Claude AI workflows
- **Git**: Automated workflow management
- **Testing**: Integrated test execution and validation

## 🔧 System Extensibility

### Adding New Claude AI Agents
```bash
# Create new specialized agents in .claude/agents/
# Follow the agent template structure
# Integrate with existing command workflows
```

### Extending GitHub Copilot Guidelines
```bash
# Add new coding guidelines in .github/coding-guidelines/
# Update copilot-instructions.md with new patterns
# Maintain consistency with Claude AI standards
```

### Custom Project Templates
- **Phoenix Patterns**: Add new framework-specific patterns
- **Industry Standards**: Adapt for specific domains or industries
- **Team Conventions**: Customize for team-specific requirements

## 🎯 Key Benefits

### For AI-Assisted Development
1. **Dual AI Power**: Combines Claude AI's planning with GitHub Copilot's coding
2. **Context Awareness**: Both systems understand Phoenix and Elixir patterns
3. **Consistency**: Unified standards across different AI interactions
4. **Efficiency**: Automated workflows reduce repetitive tasks
5. **Quality**: Built-in best practices and validation

### For Development Teams
1. **Standardization**: Consistent patterns across all projects
2. **Onboarding**: New developers can leverage existing patterns
3. **Knowledge Preservation**: Captured expertise in reusable formats
4. **Collaboration**: Structured workflows for team coordination
5. **Scalability**: Modular system grows with team needs

### For Phoenix Development
1. **Framework Expertise**: Deep Phoenix and Elixir knowledge built-in
2. **Modern Patterns**: Latest Phoenix 1.8+ patterns and best practices
3. **Security First**: Security considerations integrated throughout
4. **Performance Aware**: Performance patterns and optimizations
5. **Testing Culture**: Comprehensive testing strategies and patterns

## 🤝 Community and Contribution

This dual-AI system represents a new paradigm in software development, combining the strengths of different AI assistants for comprehensive Phoenix application development. The system is designed to evolve with the Phoenix ecosystem and AI capabilities.

### Contributing
- **Pattern Improvements**: Submit better Phoenix patterns and practices
- **AI Integration**: Enhance Claude AI and GitHub Copilot integration
- **Documentation**: Improve guides and examples
- **Tool Integration**: Add support for new development tools

## 📚 Learning Resources

### Getting Started
1. **Claude AI Setup**: Configure `.claude/` directory for your project
2. **GitHub Copilot Setup**: Enable GitHub Copilot with `.github/` guidelines
3. **Phoenix Development**: Follow the integrated development workflows
4. **Best Practices**: Study the built-in patterns and standards

### Advanced Usage
- **Custom Agents**: Create specialized Claude AI agents for specific tasks
- **Pattern Libraries**: Build reusable pattern libraries for your domain
- **Team Integration**: Adapt the system for team-specific workflows
- **Continuous Improvement**: Evolve patterns based on project experience

## Inspiration and Credits

This project was inspired by the excellent work done in [agent-os](https://github.com/buildermethods/agent-os/tree/main) by the creator at Builder Methods. Their innovative approach to AI-driven development workflows provided the foundational concepts that we've adapted and extended for Phoenix development.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### MIT License Summary

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software.

**This is completely open source software - you are free to use, modify, and distribute it as you see fit.**