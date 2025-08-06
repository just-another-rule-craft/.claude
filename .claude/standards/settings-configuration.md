# Claude Code Settings Configuration

## Overview

This document explains how to configure Claude Code settings for optimal Phoenix development workflow.

## Settings Location

Claude Code settings can be configured in several locations:
- **Project-specific**: `.claude/settings.json` (check into git for team sharing)
- **Global**: `~/.claude.json` (personal settings)
- **Session-specific**: Use `--allowedTools` CLI flag

## Recommended Tool Permissions

Use the `/permissions` command in Claude Code to add these tools:

### File Operations
```
Edit                           # Allow file editing
```

### Phoenix/Elixir Commands
```
Bash(mix test:*)              # Run tests
Bash(mix compile:*)           # Compile project
Bash(mix format:*)            # Format code
Bash(mix ecto.migrate:*)      # Database migrations
Bash(mix ecto.gen.migration:*) # Generate migrations
Bash(mix phx.gen.*:*)         # Phoenix generators
Bash(mix phx.server:*)        # Start server
```

### Git Operations
```
Bash(git add:*)               # Stage changes
Bash(git commit:*)            # Commit changes
Bash(git status:*)            # Check status
Bash(git diff:*)              # Show diffs
Bash(git log:*)               # View history
```

### GitHub CLI (if using GitHub)
```
Bash(gh pr:*)                 # Pull request operations
Bash(gh issue:*)              # Issue operations
```

### Tidewave MCP Tools
```
mcp__tidewave__project_eval           # Execute Elixir code
mcp__tidewave__execute_sql_query      # Run SQL queries
mcp__tidewave__get_docs               # Get documentation
mcp__tidewave__get_ecto_schemas       # Get schema info
mcp__tidewave__get_logs               # Access logs
mcp__tidewave__get_source_location    # Find source code
mcp__tidewave__get_package_location   # Package information
mcp__tidewave__search_package_docs    # Search docs
mcp__tidewave__list_liveview_pages    # List LiveView pages
```

## CLI Usage Examples

### Safe Mode (Default)
```bash
claude
# Prompts for permission on each tool use
```

### Pre-authorized Tools
```bash
claude --allowedTools Edit,Bash(mix test:*),Bash(git commit:*)
# Allows specific tools without prompting
```

### Dangerous Mode (Use with Caution)
```bash
claude --dangerously-skip-permissions
# Bypasses all permission checks - use only in isolated environments
```

## Configuration Commands

### Add Tools Interactively
```
/permissions
# Opens interactive permission management
```

### Check Current Permissions
```
/permissions list
# Shows currently allowed tools
```

## Domain Allowlist

For web-based tools that fetch URLs, add trusted domains:

```
/permissions add Fetch(docs.phoenixframework.org)
/permissions add Fetch(hexdocs.pm)
/permissions add Fetch(github.com)
```

## Example Settings File

If you want to share team settings, create `.claude/settings.json`:

```json
{
  "tools": {
    "allowedDomains": [
      "docs.phoenixframework.org",
      "hexdocs.pm",
      "github.com"
    ]
  }
}
```

## Security Considerations

### Safe Practices
- Use project-specific settings for team projects
- Keep dangerous commands (like `rm`, `sudo`) off the allowlist
- Use domain allowlists for URL fetching
- Review permissions regularly

### Team Sharing
- Check `.claude/settings.json` into git for team consistency
- Document any project-specific tool requirements
- Use conservative defaults for shared projects

### Development vs Production
- Never use `--dangerously-skip-permissions` on production systems
- Consider using containers for dangerous operations
- Limit database access to development databases only

## Troubleshooting

### Permission Denied Errors
1. Check current permissions: `/permissions list`
2. Add required tool: `/permissions add [tool-name]`
3. Restart Claude Code if needed

### Tool Not Found
1. Verify tool is installed: `which [tool-name]`
2. Check PATH environment variable
3. Add tool location to system PATH

### MCP Connection Issues
1. Verify Tidewave is running: `curl http://localhost:4000/tidewave/mcp`
2. Check Phoenix application status
3. Restart Phoenix application if needed

This configuration provides a secure and efficient Claude Code setup for Phoenix development while maintaining proper security boundaries.
