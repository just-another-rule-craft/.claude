# Debug Phoenix Issue

Debug a Phoenix application issue using Tidewave MCP tools and best practices.

**Usage**: `/project:debug-phoenix-issue [description of issue]`

**Arguments**:
- `$ARGUMENTS`: Description of the issue to investigate

## Workflow

Please debug the Phoenix issue: $ARGUMENTS

Follow this systematic debugging approach:

### 1. Gather Context (Explore)
- Use `get_logs` to check recent application logs for errors
- Use `get_ecto_schemas` to understand relevant data models
- Use `list_liveview_pages` to understand UI structure if UI-related
- Use `get_source_location` to find relevant code files

### 2. Plan the Investigation (Think)
Use "think hard" to analyze the gathered information and create a debugging plan:
- Identify potential root causes based on logs and error patterns
- Determine which components/modules are likely involved
- Plan a systematic approach to reproduce and isolate the issue

### 3. Reproduce and Analyze (Code)
- Use `project_eval` to reproduce the issue in a controlled manner
- Use `execute_sql_query` if database-related to explore data state
- Use `get_docs` to understand expected behavior of involved functions
- Test different scenarios to isolate the exact cause

### 4. Implement Solution (Code)
- Write failing tests that demonstrate the issue
- Implement the fix following Phoenix best practices
- Ensure the fix doesn't break existing functionality
- Run `mix compile` and `mix test` to validate the solution

### 5. Document and Commit (Commit)
- Document the root cause and solution
- Write clear commit messages explaining the fix
- Update any relevant documentation or comments

## Best Practices
- Be specific about error messages and symptoms
- Always check logs first before diving into code
- Use Tidewave tools to maintain context throughout the debugging process
- Test your solution thoroughly before considering the issue resolved
- Document lessons learned for future reference

Remember: Good debugging is systematic, not random. Follow the workflow and use the right tools for each step.
