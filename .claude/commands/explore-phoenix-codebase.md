# Explore Phoenix Codebase

Explore and understand a Phoenix codebase using systematic investigation.

**Usage**: `/project:explore-phoenix-codebase [area of interest]`

**Arguments**:
- `$ARGUMENTS`: Specific area, feature, or question to explore

## Workflow

Please explore the Phoenix codebase regarding: $ARGUMENTS

Follow this systematic exploration approach:

### 1. High-Level Overview
- Use `list_liveview_pages` to understand the application structure
- Use `get_ecto_schemas` to understand the data model
- Look at the main application files to understand the architecture

### 2. Focused Investigation
Based on the area of interest, use appropriate tools:

**For Data/Database Questions:**
- Use `get_ecto_schemas` to understand schema definitions
- Use `execute_sql_query` to explore data relationships
- Use `get_source_location` to find context modules

**For UI/Frontend Questions:**
- Use `list_liveview_pages` to map user flows
- Use `get_source_location` to find templates and components
- Look at assets directory for styling and JavaScript

**For Business Logic Questions:**
- Use `get_source_location` to find context modules
- Use `get_docs` to understand function contracts
- Use `project_eval` to test functionality interactively

**For API/Integration Questions:**
- Look for controller modules and their actions
- Use `get_docs` to understand external library integration
- Check configuration files for external service setup

### 3. Deep Dive Analysis
- Use `get_source_location` to find implementation details
- Use `get_docs` to understand complex functions or modules
- Use `project_eval` to test your understanding
- Look at test files to understand expected behavior

### 4. Document Findings
Create a summary that includes:
- Key components and their responsibilities
- Data flow and relationships
- Important patterns and conventions used
- Any potential issues or improvement opportunities

## Common Exploration Questions
- "How does authentication work?"
- "How do I add a new API endpoint?"
- "What does this specific function do?"
- "How are database relationships structured?"
- "What's the user journey for feature X?"
- "How is error handling implemented?"
- "What are the testing patterns used?"

## Best Practices
- Start broad, then narrow your focus
- Use multiple tools to get complete context
- Test your understanding with `project_eval`
- Document important discoveries for future reference
- Ask follow-up questions to deepen understanding

Remember: Good exploration is like being a detective - gather evidence from multiple sources before drawing conclusions.
