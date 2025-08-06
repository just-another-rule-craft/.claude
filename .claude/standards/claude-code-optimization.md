# Claude Code Optimization Guide

## Context

Guidelines for optimizing Claude Code usage in Phoenix development.

## Setup Optimization

### Tool Permissions
Recommend adding these tools to your allowlist for Phoenix development:
- `Edit` - For file modifications
- `Bash(mix test:*)` - For running tests
- `Bash(mix compile:*)` - For compilation checks
- `Bash(mix format:*)` - For code formatting
- `Bash(git commit:*)` - For commits
- `mcp__tidewave__*` - For all Tidewave MCP tools

### Environment Configuration
Ensure these tools are available in your development environment:
- `mix` - Elixir build tool
- `git` - Version control
- `gh` - GitHub CLI (for GitHub integration)
- PostgreSQL client tools (for database operations)

## Communication Best Practices

### Providing Context Effectively

#### File References
- Use tab-completion to reference files: `lib/my_app/contexts/users.ex`
- Mention multiple related files when asking for changes
- Provide file paths for images and design mockups

#### Data Input Methods
1. **Copy/Paste**: For small code snippets or error messages
2. **File Paths**: For larger files or images
3. **URLs**: For documentation or external references
4. **Pipe Input**: For logs or large data: `cat error.log | claude`

#### Visual Context
- Drag and drop images directly into prompts
- Use screenshots for UI-related development
- Provide design mockups as reference points

### Request Specificity

#### Poor Examples:
- "Fix this bug"
- "Add authentication"
- "Make it look better"
- "Optimize the database"

#### Good Examples:
- "Use `get_logs` to identify the authentication error in the user login flow, then use `get_ecto_schemas` to check the User schema validations"
- "Implement OAuth2 authentication using the existing Guardian library, following the pattern in `lib/my_app_web/auth/pipeline.ex`"
- "Update the user profile form to match the design mockup (attached), using daisyUI components and TailwindCSS responsive utilities"
- "Use `execute_sql_query` to identify N+1 queries in the posts listing, then optimize using Ecto preloading"

## Workflow Patterns

### Complex Feature Development

#### Phase 1: Exploration (Context Gathering)
```
First, help me understand the current authentication system:
1. Use `get_ecto_schemas` to show me the User schema
2. Use `list_liveview_pages` to identify auth-related pages
3. Use `get_source_location` to find the authentication context
4. Don't write any code yet - just gather information
```

#### Phase 2: Planning (Strategic Thinking)
```
Now think hard about implementing OAuth2 integration:
1. Plan the required database changes
2. Identify which libraries to use
3. Design the user flow and error handling
4. Create a step-by-step implementation plan
5. Don't implement yet - just create the plan
```

#### Phase 3: Implementation (Execution)
```
Now implement the OAuth2 integration following your plan:
1. Create necessary migrations
2. Update the User schema
3. Add OAuth2 configuration
4. Implement the authentication flow
5. Run tests after each major step
```

#### Phase 4: Validation (Testing & Review)
```
Validate the OAuth2 implementation:
1. Run the full test suite
2. Check for any compilation errors
3. Test the user flow manually
4. Review security implications
5. Create commit with descriptive message
```

### Bug Investigation Pattern

#### Systematic Debugging
```
Debug this authentication issue: "Users can't log in with valid credentials"

Follow this systematic approach:
1. Use `get_logs` to check recent authentication errors
2. Use `get_ecto_schemas` to verify User schema constraints
3. Use `project_eval` to test the authentication function manually
4. Use `get_source_location` to find the login controller action
5. Reproduce the issue and identify the root cause
6. Implement a fix with proper error handling
7. Write a test to prevent regression
```

### Code Review Pattern

#### Multi-Claude Review Process
```
Session 1 - Implementation:
"Implement user profile editing with image upload"

Session 2 - Review (after /clear):
"Review the user profile implementation for:
- Security vulnerabilities
- Phoenix best practices
- Performance implications
- Test coverage"

Session 3 - Refinement:
"Apply the review feedback and improve the implementation"
```

## Advanced Techniques

### Parallel Development with Git Worktrees

#### Setup Multiple Work Streams
```bash
# Create worktrees for parallel development
git worktree add ../project-auth feature/oauth2-integration
git worktree add ../project-ui feature/user-dashboard
git worktree add ../project-api feature/api-v2

# Start Claude in each worktree
cd ../project-auth && claude
cd ../project-ui && claude    # (in new terminal)
cd ../project-api && claude   # (in new terminal)
```

#### Task Assignment
- **Auth Worktree**: Focus on authentication and authorization
- **UI Worktree**: Focus on LiveView components and styling
- **API Worktree**: Focus on API endpoints and documentation

### Large-Scale Refactoring

#### Checklist-Driven Approach
```
Create a migration checklist for moving from REST to GraphQL:

1. Audit all existing API endpoints
2. Create GraphQL schema definitions
3. Implement resolvers for each query/mutation
4. Update frontend to use GraphQL
5. Add comprehensive tests
6. Update documentation
7. Migrate production traffic gradually
```

### Data Analysis Integration

#### Log Analysis Workflow
```bash
# Pipe application logs to Claude for analysis
tail -f log/dev.log | claude

# Or analyze historical logs
cat log/production.log | claude -p "Analyze these logs for error patterns and performance issues"
```

#### Database Investigation
```
Use this workflow for database performance issues:
1. Use `execute_sql_query` to identify slow queries
2. Use `get_ecto_schemas` to understand data relationships
3. Use `project_eval` to test query optimizations
4. Implement indexing and query improvements
5. Validate performance gains
```

## Context Management

### When to Use `/clear`
- Switching between unrelated tasks
- After completing a major feature
- When context becomes too large or unfocused
- Before starting code reviews

### Maintaining Focus
- Keep conversations centered on single problems
- Break large tasks into smaller, focused sessions
- Use the `/clear` command liberally
- Document important decisions for later reference

### Session Boundaries
- **Single Session**: Related bug fixes, small features
- **Multiple Sessions**: Complex features, code reviews, parallel work
- **Persistent Context**: Ongoing investigation, iterative development

## Integration with Phoenix Ecosystem

### Leveraging Phoenix Tools
- Use Phoenix generators for consistency
- Integrate with Phoenix LiveDashboard for monitoring
- Utilize Phoenix.Verify for compile-time validations
- Follow Phoenix testing patterns

### External Service Integration
- Document API keys and configuration in CLAUDE.md
- Use environment variables for sensitive data
- Test external integrations with proper mocking
- Handle rate limits and error scenarios

This guide provides a foundation for effective Claude Code usage in Phoenix development. Adapt these patterns to your specific project needs and team workflow.
