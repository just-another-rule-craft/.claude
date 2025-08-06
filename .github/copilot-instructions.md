# GitHub Copilot Instructions for Phoenix Rules

## Project Overview

Phoenix Rules framework for AI-driven development with GitHub Copilot integration.

**Purpose**: Enable efficient Phoenix development with Copilot assistance.

**Key Focus**:
- Phoenix 1.8.0-rc.4 patterns and best practices
- Elixir 1.18.1 with modern features
- LiveView for interactive UIs
- Security-first development approach

## Development Approach

### Phoenix Context
- **Framework**: Phoenix 1.8+ with LiveView
- **Database**: PostgreSQL with Ecto
- **Frontend**: TailwindCSS + daisyUI
- **Testing**: ExUnit with comprehensive coverage

### Copilot Integration
- Code completion for Phoenix patterns
- Chat assistance for complex implementations
- Automated code review with custom guidelines
- Test generation and debugging support

## Code Patterns

### Phoenix Contexts
- Organize business logic in contexts, not controllers
- Keep controllers thin - only routing and response handling
- Use pattern matching and `with` statements for error handling

### LiveView Best Practices
- Prefer LiveView for interactive UIs
- Use components for reusable UI elements
- Handle form validation with changesets
- Implement real-time features with PubSub

### Database and Ecto
- Always use changesets for data validation
- Preload associations to avoid N+1 queries
- Include proper database constraints
- Use transactions for multi-step operations

### Security Guidelines
- Sanitize all user input
- Use CSRF protection on forms
- Implement proper authentication/authorization
- Validate data at both database and application levels

## Chat Prompts

### Effective Copilot Chat Usage

**For Code Review**:
```
Review this Phoenix controller for best practices:
[paste code]

Focus on: error handling, security, performance
```

**For Implementation**:
```
Create a Phoenix LiveView for user registration with:
- Email/password validation
- Real-time form feedback
- daisyUI styling
- Error handling
```

**For Debugging**:
```
Debug this Ecto query performance issue:
[paste query]

Suggest optimizations and alternatives
```

## Integration with .claude Structure

This GitHub Copilot configuration complements the existing `.claude` directory:

- **`.claude/`**: Claude-specific instructions and complex analysis
- **`.github/`**: GitHub Copilot automation and daily development
- **Shared Goals**: Security, performance, testing, documentation

Both systems work together for comprehensive AI-assisted Phoenix development.
```
