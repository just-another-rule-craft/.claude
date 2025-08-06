# Development Best Practices

## Context

Global development guidelines.

<conditional-block context-check="core-principles">
IF this Core Principles section already read in current context:
  SKIP: Re-reading this section
  NOTE: "Using Core Principles already in context"
ELSE:
  READ: The following principles

## Core Principles

### Keep It Simple
- Implement code in the fewest lines possible
- Avoid over-engineering solutions
- Choose straightforward approaches over clever ones

### Optimize for Readability
- Prioritize code clarity over micro-optimizations
- Write self-documenting code with clear variable names
- Add comments for "why" not "what"

### DRY (Don't Repeat Yourself)
- Extract repeated business logic to private methods
- Extract repeated UI markup to reusable components
- Create utility functions for common operations

### File Structure
- Keep files focused on a single responsibility
- Group related functionality together
- Use consistent naming conventions
</conditional-block>


<conditional-block context-check="tidewave-mcp-tools" task-condition="using-ai-assistance">
IF current task involves AI-assisted development or debugging:
  IF Tidewave MCP Tools section already read in current context:
    SKIP: Re-reading this section
    NOTE: "Using Tidewave MCP guidelines already in context"
  ELSE:
    READ: The following Tidewave MCP usage guidelines
ELSE:
  SKIP: Tidewave MCP tools section not relevant to current task

## Tidewave MCP Tools

### Overview
Tidewave provides powerful MCP (Model Context Protocol) tools that allow AI assistants to interact directly with your Phoenix application runtime. These tools enable more precise and contextual assistance.

### Core Tools and Their Best Uses

#### `project_eval` - The Swiss Army Knife
- **Use for**: Executing Elixir code within your project context
- **Best practices**:
  - Be specific about what you want to evaluate
  - Use for complex data transformations and business logic validation
  - Leverage existing functions and modules in your codebase
  - Ask AI to validate APIs or test specific functionality

#### `execute_sql_query` - Direct Database Access
- **Use for**: Running SQL queries against your development database
- **Best practices**:
  - Prefer structured data loading through your Ecto schemas when possible
  - Use for debugging database issues and exploring data
  - Combine with `get_ecto_schemas` for context-aware queries
  - Be explicit about which tables/data you want to query

#### `get_docs` - Runtime Documentation
- **Use for**: Retrieving documentation for functions, modules, and types
- **Best practices**:
  - Use when you need to understand API contracts
  - Combine with source location tools for complete context
  - Helpful for exploring third-party library documentation

#### `get_ecto_schemas` - Database Schema Context
- **Use for**: Understanding your application's data models
- **Best practices**:
  - Use before writing database queries or migrations
  - Essential for understanding relationships between entities
  - Combine with `execute_sql_query` for data exploration

#### `get_logs` - Application Debugging
- **Use for**: Debugging runtime issues and understanding application behavior
- **Best practices**:
  - Use when troubleshooting errors or unexpected behavior
  - Combine with code evaluation to reproduce and fix issues
  - Essential for production debugging workflows

#### `get_source_location` - Code Navigation
- **Use for**: Finding where functions, modules, or macros are defined
- **Best practices**:
  - Use when you need to understand implementation details
  - Helpful for refactoring and code exploration
  - Combine with `get_docs` for complete understanding

#### `get_package_location` - Dependency Management
- **Use for**: Understanding where dependencies are installed and their structure
- **Best practices**:
  - Use when debugging dependency conflicts
  - Helpful for understanding third-party library structure
  - Essential for contributing to open source projects

#### `search_package_docs` - Documentation Search
- **Use for**: Finding specific documentation across your dependencies
- **Best practices**:
  - Use when you know what you're looking for but not where
  - More efficient than manual documentation browsing
  - Combine with other tools for comprehensive understanding

#### `list_liveview_pages` - UI Context
- **Use for**: Understanding your application's LiveView structure
- **Best practices**:
  - Use when planning UI changes or new features
  - Essential for understanding the user journey
  - Combine with source location tools for implementation details

### Integration Best Practices

#### Be Specific with Tool Requests
- Instead of: "Help me debug this issue"
- Use: "Use `get_logs` to check recent errors, then `project_eval` to reproduce the issue"

#### Combine Tools for Complete Context
- Use `get_ecto_schemas` + `execute_sql_query` for database work
- Use `get_source_location` + `get_docs` for code understanding
- Use `list_liveview_pages` + `get_source_location` for UI development

#### Leverage Your Existing Codebase
- Use `project_eval` to call your existing functions rather than rewriting logic
- Ask AI to use your established patterns and conventions
- Utilize your authentication and API access patterns through eval

#### Configure AI Assistant Prompts
Include Tidewave tool preferences in your AI assistant configuration:
```
This is a Phoenix application with Tidewave MCP integration.
When debugging, prefer using get_logs and project_eval.
For database work, use get_ecto_schemas and execute_sql_query.
For code exploration, use get_source_location and get_docs.
Always be specific about which Tidewave tools to use.
```
</conditional-block>

<conditional-block context-check="claude-code-workflows" task-condition="using-claude-code">
IF current task involves using Claude Code for development:
  IF Claude Code Workflows section already read in current context:
    SKIP: Re-reading this section
    NOTE: "Using Claude Code workflows already in context"
  ELSE:
    READ: The following Claude Code workflow guidelines
ELSE:
  SKIP: Claude Code workflows section not relevant to current task

## Claude Code Workflows

### Core Development Patterns

#### Explore, Plan, Code, Commit
**Best for**: Complex features requiring deep understanding
1. **Explore**: Read relevant files, use Tidewave tools to understand context
2. **Plan**: Use "think hard" to create detailed implementation strategy
3. **Code**: Implement solution following the plan
4. **Commit**: Create descriptive commits and documentation

#### Test-Driven Development (TDD)
**Best for**: Features with clear input/output requirements
1. **Write Tests**: Create failing tests for expected behavior
2. **Commit Tests**: Save tests before implementation
3. **Code**: Write minimal code to pass tests
4. **Iterate**: Refine until all tests pass
5. **Commit**: Save working implementation

#### Visual Development
**Best for**: UI/UX features with design mocks
1. **Provide Visual Target**: Share design mockups or screenshots
2. **Implement**: Create initial implementation
3. **Screenshot**: Take screenshots of current state
4. **Iterate**: Compare with target and refine
5. **Commit**: Save polished implementation

### Workflow Optimization

#### Be Specific in Instructions
- ❌ Poor: "Fix the user authentication"
- ✅ Good: "Use `get_logs` to identify authentication errors, then `get_ecto_schemas` to understand the User schema, and fix the password validation in the login context"

#### Use Planning Before Coding
- Always ask Claude to make a plan before implementing
- Use "think", "think hard", or "think harder" for complex problems
- Confirm the plan looks good before proceeding to implementation

#### Course Correct Early and Often
- Use Escape to interrupt Claude during any phase
- Double-tap Escape to edit previous prompts and explore different directions
- Ask Claude to undo changes when taking a different approach

#### Maintain Focused Context
- Use `/clear` command between unrelated tasks
- Keep conversations focused on single problems
- Reset context when switching to different areas of the codebase

### Multi-Claude Strategies

#### Code Review Pattern
1. Have one Claude write code
2. Use `/clear` or start new session
3. Have second Claude review the first Claude's work
4. Use third Claude to implement review feedback

#### Parallel Development
- Use git worktrees for independent features
- Run multiple Claude sessions on different tasks
- Each Claude works on isolated branches/features

#### Verification Workflow
- One Claude implements feature
- Second Claude writes comprehensive tests
- Third Claude validates the complete solution

### Custom Commands Usage
Use the available slash commands for common workflows:
- `/project:debug-phoenix-issue` - Systematic debugging approach
- `/project:implement-phoenix-feature` - TDD feature implementation
- `/project:explore-phoenix-codebase` - Codebase investigation
- `/project:phoenix-code-review` - Comprehensive code review

### Advanced Techniques

#### Checklists for Complex Tasks
For large migrations or multi-step processes:
1. Have Claude create a Markdown checklist
2. Work through items systematically
3. Check off completed items
4. Maintain focus on current task

#### Data-Driven Development
- Pipe data into Claude: `cat logs.txt | claude`
- Use Claude to analyze patterns in logs, CSVs, or large datasets
- Combine with Tidewave tools for context-aware analysis

#### Iterative Improvement
- First implementation doesn't need to be perfect
- Use 2-3 iterations to refine and improve
- Each iteration should build on previous learnings
</conditional-block>