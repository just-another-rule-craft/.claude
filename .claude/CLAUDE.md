# Phoenix Development with Claude Code

## Advanced Slash Commands (Use These First)

### Phoenix Development Workflows
- `/project:debug-phoenix-issue [specific issue]` - Systematic debugging with Tidewave integration
- `/project:implement-phoenix-feature [feature description]` - Complete feature implementation workflow
- `/project:phoenix-code-review [files/component]` - Comprehensive code review process
- `/project:api-dev [api task]` - API development, documentation, and testing
- `/project:deploy-monitor [deployment task]` - Production deployment and monitoring

### Advanced Operations
- `/project:advanced-debug [complex debugging scenario]` - Multi-layer debugging with performance analysis
- `/project:optimize-performance [performance issue]` - Systematic performance optimization
- `/project:security-audit [security concern]` - Comprehensive security vulnerability assessment
- `/project:manage-migrations [migration task]` - Safe database migration management
- `/project:testing-strategy [testing approach]` - Complete testing strategy implementation

## Essential Phoenix Commands

### Development Workflow
```bash
mix phx.server              # Start Phoenix development server
mix test                    # Run full test suite
mix test test/specific_test.exs  # Run specific test file
mix test --failed           # Re-run only failed tests
mix format                  # Format code (auto-run before commits)
mix compile --warnings-as-errors  # Strict compilation
```

### Database Operations
```bash
mix ecto.migrate            # Apply migrations
mix ecto.rollback           # Rollback last migration
mix ecto.gen.migration name # Generate migration
mix ecto.reset              # Drop, create, migrate, seed database
```

### Phoenix Generators
```bash
mix phx.gen.live Context Schema table field:type    # Generate LiveView CRUD
mix phx.gen.context Context Schema table field:type # Generate context
mix phx.gen.auth Accounts User users                 # Generate authentication
```

## Claude Code Best Practices

### Code Quality Standards
- **ALWAYS** use pattern matching in function heads and case statements
- **PREFER** LiveView over traditional controllers for interactive UIs
- **ORGANIZE** business logic in Phoenix contexts, NOT in controllers
- **USE** descriptive variable names in snake_case (user_params, not up)
- **DOCUMENT** all public functions with @doc and @spec
- **HANDLE** errors explicitly with with statements or case patterns
- **AVOID** nested conditionals - use with or early returns instead

### Development Workflow (MANDATORY)
1. **Analysis Phase**: Use "think hard" to understand requirements completely
2. **Planning Phase**: Create step-by-step implementation plan
3. **Context Gathering**: Use Tidewave tools to gather necessary context
4. **Implementation**: Write code following Phoenix patterns
5. **Testing**: Comprehensive test coverage with proper fixtures
6. **Validation**: Run tests, compile checks, and format code
7. **Documentation**: Update relevant documentation

### Thinking Instructions (Critical)
- Use **"think hard"** for architectural decisions and complex problems
- Use **"think step by step"** for debugging and multi-step processes
- Use **"ultrathink"** only for critical system design choices
- **ALWAYS** plan before implementing - confirm approach first
- **BREAK DOWN** complex tasks into manageable phases

## Tidewave MCP Tools Integration

### Core Tools (Use These Systematically)
- `project_eval` - Execute Elixir code and evaluate expressions
- `execute_sql_query` - Run SQL queries against the database
- `get_docs` - Retrieve documentation for modules and functions
- `get_ecto_schemas` - Get Ecto schema definitions and relationships
- `get_logs` - Fetch application logs for debugging
- `get_source_location` - Find source code location of modules/functions
- `get_package_location` - Locate package dependencies
- `search_package_docs` - Search through package documentation
- `list_liveview_pages` - List all LiveView pages in the application

### Usage Patterns (BE SPECIFIC)
**For debugging**:
```
1. Use `get_logs` to check recent errors
2. Use `project_eval` to reproduce the issue
3. Use `get_source_location` to find relevant code
4. Use `get_docs` for function documentation
```

**For database work**:
```
1. Use `get_ecto_schemas` to understand data structure
2. Use `execute_sql_query` to verify data integrity
3. Use `project_eval` to test Ecto queries
```

**For code exploration**:
```
1. Use `get_source_location` to find modules
2. Use `get_docs` for function documentation
3. Use `search_package_docs` for dependency usage
4. Use `list_liveview_pages` for UI components
```

## Technical Environment

### Tech Stack (Phoenix 1.8+ Modern Stack)
- **Phoenix Framework**: 1.8.0-rc.4 with latest features
- **Elixir**: 1.18.1 with OTP 27+ for optimal performance
- **Database**: PostgreSQL 17+ with advanced features
- **Styling**: TailwindCSS 4.0+ with modern utility classes
- **UI Components**: daisyUI 5.0+ for consistent design system
- **Real-time**: Phoenix LiveView for interactive UIs
- **Testing**: ExUnit with comprehensive test patterns

### Dependencies & Assets
```bash
mix deps.get                # Fetch dependencies
mix deps.compile            # Compile dependencies
mix assets.deploy           # Build production assets
mix assets.setup            # Setup asset pipeline
```

## Testing Strategy (Comprehensive)

### Test-Driven Development
- **Write tests first**: Create failing tests for new functionality
- **Red-Green-Refactor**: Follow TDD cycle religiously
- **Test structure**: Use Arrange-Act-Assert pattern consistently
- **Test naming**: Descriptive names explaining expected behavior
- **Test data**: Use factories and fixtures for consistent data

### Test Types and Coverage
```bash
mix test                    # Run all tests
mix test --cover            # Generate coverage report
mix test --failed           # Re-run only failed tests
mix test.watch              # Continuous testing during development
mix test test/specific_test.exs  # Run specific test file
```

### Testing Patterns
- **Unit tests**: Test individual functions and modules
- **Integration tests**: Test complete workflows
- **LiveView tests**: Test user interactions and real-time features
- **API tests**: Test endpoint behavior and responses
- **Database tests**: Test data integrity and constraints

## Security Best Practices

### Input Validation and Sanitization
- **Validate at boundaries**: Always validate user input at context layer
- **Use changesets**: Leverage Ecto changesets for data validation
- **Sanitize HTML**: Strip dangerous HTML from user content
- **SQL injection prevention**: Use Ecto queries, never raw SQL with user input

### Authentication and Authorization
- **Use Phoenix.LiveView.Router**: Secure LiveView routes properly
- **Implement proper sessions**: Secure session management
- **CSRF protection**: Enable and configure CSRF tokens
- **Rate limiting**: Implement API rate limiting for security

## Performance Optimization

### Database Performance
- **Query optimization**: Use Ecto query analysis tools
- **N+1 prevention**: Proper preloading of associations
- **Database indexing**: Strategic index creation for queries
- **Connection pooling**: Optimize database connection pools

### Application Performance
- **LiveView optimization**: Minimize state and optimize updates
- **Caching strategies**: Implement appropriate caching layers
- **Asset optimization**: Optimize CSS/JS bundle sizes
- **Memory management**: Monitor and optimize memory usage

## Git Workflow and Version Control

### Repository Management
- **Commit messages**: Use conventional commit format
- **Branch naming**: Use descriptive branch names (feature/*, fix/*, etc.)
- **Code reviews**: Mandatory reviews before merging
- **Documentation**: Update docs with API changes

### Commit Standards
```bash
git commit -m "feat(auth): add user authentication with LiveView"
git commit -m "fix(api): resolve validation error in user creation"
git commit -m "docs(readme): update installation instructions"
git commit -m "test(users): add comprehensive user context tests"
```

## Error Handling and Debugging

### Error Handling Patterns
- **Use with statements**: For complex error chains
- **Pattern matching**: Match on specific error conditions
- **Fallback controllers**: Implement comprehensive API error handling
- **User-friendly errors**: Display meaningful error messages

### Debugging Workflow
1. **Check logs**: Use `get_logs` to identify error patterns
2. **Reproduce issue**: Use `project_eval` to recreate problems
3. **Inspect code**: Use `get_source_location` to find relevant code
4. **Verify fixes**: Run comprehensive tests after fixes

## Production Deployment

### Release Management
- **Elixir releases**: Use modern release tooling
- **Environment configuration**: Proper runtime configuration
- **Health checks**: Implement comprehensive health endpoints
- **Monitoring**: Set up application and infrastructure monitoring

### Deployment Checklist
- [ ] All tests passing
- [ ] Security audit completed
- [ ] Performance benchmarks met
- [ ] Documentation updated
- [ ] Monitoring configured
- [ ] Rollback procedures tested

## Project-Specific Guidelines

### Phoenix Development Patterns
- **LiveView first**: Prefer LiveView over traditional controllers
- **Context organization**: Keep business logic in contexts
- **Minimal controllers**: Controllers should be thin routing layers
- **Proper supervision**: Use supervision trees for fault tolerance

### Code Organization
- **Consistent naming**: Follow Elixir/Phoenix naming conventions
- **Module organization**: Group related functionality logically
- **Documentation**: Comprehensive @doc and @spec annotations
- **Type specifications**: Use @spec for public functions

### Common Pitfalls to Avoid
- **Heavy LiveView state**: Keep LiveView assigns minimal
- **Circular dependencies**: Avoid circular context dependencies
- **Unsafe migrations**: Ensure all migrations are reversible
- **N+1 queries**: Always check for query optimization opportunities

## .claude Directory Structure

The `.claude` system provides a complete organizational framework for AI-driven Phoenix development following Claude Code best practices:

```
.claude/
├── CLAUDE.md                  # Main configuration (this file)
├── best-practices.md          # Core development guidelines with conditional blocks
├── tidewave.md               # Comprehensive Tidewave MCP tools documentation
│
├── agents/                   # Specialized AI agents for specific tasks
│   ├── context-fetcher.md    # Intelligent context retrieval and analysis
│   ├── file-creator.md       # File and template creation with Phoenix patterns
│   ├── git-workflow.md       # Git workflow management and automation
│   ├── test-runner.md        # Test execution, validation, and reporting
│   └── refactor-agent.md     # Code refactoring and modernization specialist
│
├── commands/                 # High-level slash commands for development workflows
│   ├── debug-phoenix-issue.md     # Systematic debugging with Tidewave integration
│   ├── implement-phoenix-feature.md # Complete feature implementation workflow
│   ├── phoenix-code-review.md     # Comprehensive code review process
│   ├── api-dev.md                 # API development, documentation, and testing
│   ├── deploy-monitor.md          # Production deployment and monitoring
│   ├── advanced-debug.md          # Multi-layer debugging with performance analysis
│   ├── optimize-performance.md    # Systematic performance optimization
│   ├── security-audit.md          # Security vulnerability assessment
│   ├── manage-migrations.md       # Safe database migration management
│   └── testing-strategy.md        # Comprehensive testing strategy implementation
│
├── instructions/             # Detailed operational instructions
│   ├── configuration.instructions.md    # Configuration file guidelines
│   ├── frontend.instructions.md         # Frontend development instructions
│   ├── migrations.instructions.md       # Database migration best practices
│   ├── phoenix-development.instructions.md # Core Phoenix development patterns
│   ├── phoenix-web.instructions.md      # Phoenix web layer instructions
│   └── testing.instructions.md          # Testing strategy and implementation
│
├── libraries_docs/           # Third-party library documentation and integration guides
├── phoenix_guide/            # Phoenix framework-specific comprehensive guides
│   ├── controllers.md        # Controller patterns and best practices
│   ├── security.md          # Security implementation guidelines
│   ├── data_modelling/       # Data modeling patterns and strategies
│   │   ├── contexts.md       # Phoenix context patterns and organization
│   │   ├── schemas.md        # Ecto schema patterns and relationships
│   │   └── migrations.md     # Database migration patterns and safety
│   ├── deployment/           # Production deployment strategies
│   │   ├── docker.md         # Docker configuration and containerization
│   │   ├── releases.md       # Elixir releases and production setup
│   │   └── monitoring.md     # Application monitoring and observability
│   └── howto/               # Specific implementation guides and tutorials
│       ├── authentication.md # User authentication and authorization
│       ├── real-time.md      # Phoenix LiveView and Channels patterns
│       └── testing.md        # Testing strategies and best practices
│
└── standards/               # Development standards and conventions
    ├── best-practices.md    # General development best practices
    ├── code-style.md       # Overall code style guidelines
    ├── tech-stack.md       # Default technology stack and tooling
    └── code-style/         # Language-specific style guides
        ├── css-style.md    # CSS, TailwindCSS, and daisyUI conventions
        ├── html-style.md   # HTML template and LiveView conventions
        └── javascript-style.md # JavaScript/TypeScript conventions
```

## Usage Instructions

### Slash Commands (Primary Interface)
Use the slash commands as your primary interface for development workflows:
- Start with `/project:debug-phoenix-issue` for any problems
- Use `/project:implement-phoenix-feature` for new features
- Apply `/project:phoenix-code-review` before finalizing code
- Leverage other specialized commands as needed

### Best Practices Integration
- All commands automatically integrate Tidewave MCP tools
- Follow the systematic phase-based approach in each command
- Use "think hard" instructions embedded in workflows
- Refer to best-practices.md for conditional development guidelines

### Context Management
- The system uses conditional blocks to provide relevant context
- Instructions adapt based on file types and development phases
- Agents coordinate to provide comprehensive development support

This structure ensures consistent, high-quality Phoenix development following modern best practices and Claude Code guidelines.
