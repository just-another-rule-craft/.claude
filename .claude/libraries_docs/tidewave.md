# Tidewave - Phoenix AI Development Assistant

## Overview

Tidewave is a development tool that connects your Phoenix application runtime with AI assistants through the Model Context Protocol (MCP). It provides powerful tools that allow AI to understand and interact with your running application.

## Installation

### Using Igniter (Recommended)

```bash
# Install igniter_new if you haven't already
mix archive.install hex igniter_new

# Install tidewave
mix igniter.install tidewave
```

### Manual Installation

Add to your `mix.exs`:

```elixir
def deps do
  [
    {:tidewave, "~> 0.2", only: :dev}
  ]
end
```

Then add to your `lib/my_app_web/endpoint.ex` above the `if code_reloading? do` block:

```elixir
if Code.ensure_loaded?(Tidewave) do
  plug Tidewave
end

if code_reloading? do
  # existing code...
end
```

## MCP Configuration

Tidewave runs on the same port as your Phoenix application. The MCP endpoint is available at:
`http://localhost:4000/tidewave/mcp`

Configure your AI assistant to use this as an MCP server of type "sse".

## Available Tools

### Runtime Intelligence Tools

#### `project_eval`
**Purpose**: Execute Elixir code within your project context
**Best Use Cases**:
- Validate API endpoints and business logic
- Test complex data transformations
- Debug issues by reproducing them in context
- Execute custom functions from your codebase

**Usage Tips**:
- Be specific about what you want to evaluate
- Leverage existing functions rather than rewriting logic
- Use for complex operations that benefit from your app's context

#### `execute_sql_query`
**Purpose**: Run SQL queries directly against your development database
**Best Use Cases**:
- Debug database issues
- Explore data relationships
- Test query performance
- Validate data migrations

**Usage Tips**:
- Combine with `get_ecto_schemas` for context-aware queries
- Prefer Ecto schema operations when possible
- Be explicit about which tables/data you want to query

#### `get_docs`
**Purpose**: Retrieve documentation for functions, modules, and types
**Best Use Cases**:
- Understand API contracts and function signatures
- Explore third-party library documentation
- Get context about unfamiliar code

**Usage Tips**:
- Use when you need to understand how to use specific functions
- Combine with `get_source_location` for complete context
- Helpful for exploring new dependencies

#### `get_ecto_schemas`
**Purpose**: Understand your application's data models and relationships
**Best Use Cases**:
- Before writing database queries or migrations
- Understanding entity relationships
- Planning data model changes

**Usage Tips**:
- Essential before any database-related work
- Use to understand field types and validations
- Combine with `execute_sql_query` for data exploration

#### `get_logs`
**Purpose**: Access application logs for debugging
**Best Use Cases**:
- Troubleshooting runtime errors
- Understanding application behavior
- Debugging integration issues

**Usage Tips**:
- Use when you encounter unexpected behavior
- Combine with `project_eval` to reproduce and fix issues
- Essential for production debugging workflows

#### `get_source_location`
**Purpose**: Find where functions, modules, or macros are defined
**Best Use Cases**:
- Code navigation and exploration
- Understanding implementation details
- Refactoring assistance

**Usage Tips**:
- Use when you need to see actual implementation
- Helpful for understanding complex behavior
- Combine with `get_docs` for complete understanding

#### `get_package_location`
**Purpose**: Understand dependency structure and installation paths
**Best Use Cases**:
- Debugging dependency conflicts
- Understanding third-party library structure
- Contributing to open source projects

**Usage Tips**:
- Use when dependency behavior is unclear
- Helpful for understanding how libraries are organized
- Essential for complex dependency debugging

#### `search_package_docs`
**Purpose**: Search documentation across all your dependencies
**Best Use Cases**:
- Finding specific functionality in dependencies
- Discovering available functions and modules
- Understanding library capabilities

**Usage Tips**:
- More efficient than manual documentation browsing
- Use when you know what you need but not where to find it
- Combine with other tools for comprehensive understanding

#### `list_liveview_pages`
**Purpose**: Get an overview of your application's LiveView structure
**Best Use Cases**:
- Planning UI changes or new features
- Understanding user journey and page flow
- LiveView-specific development

**Usage Tips**:
- Use when working on UI/UX improvements
- Essential for understanding application structure
- Combine with source location tools for implementation details

## Best Practices

### Tool Combination Strategies

#### Database Development Workflow
1. Use `get_ecto_schemas` to understand data models
2. Use `execute_sql_query` to explore existing data
3. Use `project_eval` to test Ecto operations
4. Use `get_logs` to debug any issues

#### Debugging Workflow
1. Use `get_logs` to identify error patterns
2. Use `get_source_location` to find relevant code
3. Use `project_eval` to reproduce the issue
4. Use `get_docs` to understand expected behavior

#### Feature Development Workflow
1. Use `list_liveview_pages` to understand current UI structure
2. Use `get_ecto_schemas` to understand data requirements
3. Use `get_source_location` to find similar implementations
4. Use `project_eval` to test new functionality

### AI Assistant Configuration

Include these preferences in your AI assistant's system prompt:

```
This Phoenix application uses Tidewave MCP for enhanced development assistance.

Tool Usage Preferences:
- For debugging: Use get_logs, then project_eval to reproduce issues
- For database work: Start with get_ecto_schemas, then execute_sql_query
- For code exploration: Use get_source_location and get_docs together
- For UI development: Use list_liveview_pages and get_source_location

Always be specific about which Tidewave tools to use and why.
```

### Communication Best Practices

#### Be Specific with Requests
❌ Poor: "Help me debug this issue"
✅ Good: "Use `get_logs` to check for recent errors, then `project_eval` to reproduce the authentication issue"

#### Request Tool Combinations
❌ Poor: "Show me the user schema"
✅ Good: "Use `get_ecto_schemas` to show the User schema, then `get_source_location` to find its implementation"

#### Leverage Your Codebase
❌ Poor: "Write a function to validate emails"
✅ Good: "Use `project_eval` to test our existing email validation function with different inputs"

## Configuration Options

### Basic Configuration

```elixir
# In your endpoint.ex
plug Tidewave, [
  allowed_origins: ["http://localhost:3000"],
  allow_remote_access: false,
  autoformat: true
]
```

### Tool Filtering

```elixir
# Include only specific tools
plug Tidewave, tools: [include: [:project_eval, :get_docs, :execute_sql_query]]

# Exclude specific tools
plug Tidewave, tools: [exclude: [:execute_sql_query]]
```

### Inspect Options

```elixir
plug Tidewave, inspect_opts: [
  charlists: :as_lists,
  limit: 100,
  pretty: true
]
```

## Security Considerations

- Tidewave only allows localhost connections by default
- Use `allowed_origins` for browser-based MCP connections
- Only enable `allow_remote_access` in trusted networks
- Consider tool filtering in production-like environments
- SQL queries are executed with your application's database permissions

## Troubleshooting

### Common Issues

1. **MCP Connection Failed**
   - Ensure Phoenix application is running
   - Verify the MCP URL: `http://localhost:4000/tidewave/mcp`
   - Check that Tidewave plug is properly configured

2. **Tools Not Available**
   - Verify Tidewave is added to your dev dependencies
   - Ensure the plug is added to your endpoint
   - Check tool filtering configuration

3. **SQL Queries Failing**
   - Ensure database is properly configured and running
   - Verify database credentials in your dev config
   - Check that migrations are up to date

### Debug Mode

Enable debug logging to troubleshoot issues:

```elixir
config :logger, level: :debug
```

This will show detailed information about MCP tool execution and any errors.
