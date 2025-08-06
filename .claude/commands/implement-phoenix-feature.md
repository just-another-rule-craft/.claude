# Implement Phoenix Feature

Implement a new feature in a Phoenix application using TDD and best practices.

**Usage**: `/project:implement-phoenix-feature [feature description]`

**Arguments**:
- `$ARGUMENTS`: Description of the feature to implement

## Workflow

Please implement the Phoenix feature: $ARGUMENTS

Follow this Test-Driven Development workflow:

### 1. Explore and Understand (Explore)
- Use `get_ecto_schemas` to understand existing data models
- Use `list_liveview_pages` to understand current UI structure
- Use `get_source_location` to find similar existing implementations
- Use `get_docs` to understand relevant Phoenix/Ecto APIs

### 2. Plan the Implementation (Think Hard)
Create a detailed implementation plan:
- Define the data requirements (new schemas, migrations, contexts)
- Plan the UI components (LiveViews, templates, components)
- Identify required business logic and validations
- Consider security implications and edge cases
- Plan the testing strategy

### 3. Write Tests First (TDD)
- Write failing tests for the expected behavior
- Include unit tests for business logic
- Include integration tests for user workflows
- Use descriptive test names that explain expected behavior
- Run tests to confirm they fail: `mix test`

### 4. Implement the Feature
Follow Phoenix conventions:
- Create database migrations if needed: `mix ecto.gen.migration`
- Define Ecto schemas with proper validations
- Implement business logic in Phoenix contexts
- Create LiveView modules for interactive UI
- Use TailwindCSS and daisyUI for styling
- Follow the Explore → Plan → Code → Test cycle

### 5. Iterate Until Complete
- Run `mix test` frequently to validate progress
- Use `project_eval` to test business logic manually
- Use `execute_sql_query` to verify database operations
- Refactor code for clarity and maintainability
- Ensure all tests pass before moving forward

### 6. Final Validation and Documentation
- Run full test suite: `mix test`
- Check compilation: `mix compile`
- Format code: `mix format`
- Update documentation and comments
- Create descriptive commit messages

## Best Practices
- Always start with tests - they guide your implementation
- Use Phoenix generators when appropriate for consistency
- Follow RESTful conventions for routes and controllers
- Implement proper error handling and validation
- Use LiveView for interactive features
- Keep contexts focused and well-defined
- Test both happy path and edge cases

Remember: TDD with Phoenix is powerful because tests provide immediate feedback on your design decisions.
