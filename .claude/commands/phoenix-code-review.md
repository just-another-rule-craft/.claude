# Phoenix Code Review

Perform a comprehensive code review of Phoenix application changes.

**Usage**: `/project:phoenix-code-review [files or description]`

**Arguments**:
- `$ARGUMENTS`: Specific files to review or description of changes

## Workflow

Please perform a code review for: $ARGUMENTS

Follow this systematic review approach:

### 1. Understand the Changes
- Read the files or changes mentioned
- Use `get_source_location` to understand context
- Use `get_docs` to verify API usage
- Use `get_ecto_schemas` if data models are involved

### 2. Check Phoenix Best Practices

**Architecture & Design:**
- Are Phoenix contexts properly used for business logic?
- Is the separation between web and business logic clear?
- Are LiveViews used appropriately for interactive features?
- Is the code following Phoenix conventions?

**Data Layer:**
- Are Ecto schemas properly defined with validations?
- Are database operations efficient (no N+1 queries)?
- Are migrations reversible and safe?
- Is proper error handling implemented?

**Web Layer:**
- Are routes following RESTful conventions?
- Are controllers thin and focused?
- Are templates and components properly organized?
- Is CSRF protection and security properly handled?

### 3. Code Quality Review

**Elixir Best Practices:**
- Is pattern matching used effectively?
- Are functions pure where possible?
- Is error handling comprehensive (using with, case, try/rescue)?
- Are guards used appropriately?
- Is the code documented with @doc and @spec?

**Testing:**
- Are tests comprehensive and well-structured?
- Do tests follow the Arrange-Act-Assert pattern?
- Are both success and failure scenarios tested?
- Are test names descriptive?

**Performance & Security:**
- Are there potential performance bottlenecks?
- Is user input properly validated and sanitized?
- Are authentication and authorization properly implemented?
- Are secrets and configuration properly managed?

### 4. Use Tools for Verification
- Use `project_eval` to test code behavior and edge cases
- Use `execute_sql_query` to verify database operations and constraints
- Use `get_logs` to check for any errors, warnings, or performance issues
- Run tests with `mix test` and check for compilation warnings
- Use `mix credo` for code quality analysis
- Use `mix dialyzer` for type checking (if configured)

### 5. Provide Structured Feedback
Structure your review with clear priorities:

**🟢 Strengths**: What's implemented well
- Good use of Phoenix conventions
- Effective error handling patterns
- Comprehensive test coverage
- Clear and readable code

**🔴 Critical Issues**: Must fix before deployment
- Security vulnerabilities
- Data corruption risks
- Performance bottlenecks
- Breaking changes

**🟡 Suggestions**: Improvements and optimizations
- Code organization enhancements
- Performance optimizations
- Maintainability improvements
- Documentation updates

**🔵 Security Concerns**: Security-related observations
- Input validation gaps
- Authorization issues
- Data exposure risks
- Audit trail requirements

**⚡ Performance Issues**: Performance-related findings
- N+1 query problems
- Memory usage concerns
- Inefficient algorithms
- Scalability limitations

### 6. Phoenix-Specific Review Focus

**Context Layer Review**:
```
Use `get_source_location` to examine:
- Business logic properly encapsulated in contexts
- Pure functions without side effects where possible
- Proper error handling with {:ok, result} | {:error, reason}
- Appropriate use of Ecto.Multi for transactions
- Database operations wrapped in contexts, not exposed in web layer
```

**Web Layer Review**:
```
Check controllers and LiveViews for:
- Thin controllers that delegate to contexts
- Proper parameter validation using changesets
- CSRF protection enabled
- Authentication and authorization at appropriate levels
- LiveView state management (minimal assigns, proper lifecycle)
```

**Database Layer Review**:
```
Use `get_ecto_schemas` and examine:
- Schema validations are comprehensive
- Associations properly defined with foreign keys
- Unique constraints where appropriate
- Database indexes for performance
- Migration safety (reversible, no data loss)
```

## Review Checklist

**Code Style:**
- [ ] Follows Elixir naming conventions
- [ ] Proper indentation and formatting
- [ ] Clear and descriptive variable names
- [ ] Appropriate use of comments

**Phoenix Patterns:**
- [ ] Proper use of contexts and schemas
- [ ] Controller actions are thin
- [ ] LiveView state management is minimal
- [ ] Proper error handling in web layer

**Testing:**
- [ ] Tests exist for new functionality
- [ ] Tests are well-structured and descriptive
- [ ] Edge cases are covered
- [ ] Test data is properly set up

**Security:**
- [ ] Input validation is comprehensive
- [ ] SQL injection prevention
- [ ] XSS prevention
- [ ] Proper authentication/authorization

## Best Practices
- Be constructive and specific in feedback
- Suggest improvements with examples
- Prioritize issues by severity
- Check for common Phoenix pitfalls
- Verify testing coverage

Remember: Good code reviews improve code quality and help team members learn.
