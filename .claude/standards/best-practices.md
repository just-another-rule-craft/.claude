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

<conditional-block context-check="dependencies" task-condition="choosing-external-library">
IF current task involves choosing an external library:
  IF Dependencies section already read in current context:
    SKIP: Re-reading this section
    NOTE: "Using Dependencies guidelines already in context"
  ELSE:
    READ: The following guidelines
ELSE:
  SKIP: Dependencies section not relevant to current task

## Dependencies

### Choose Hex Packages Wisely
When adding third-party dependencies to your Elixir/Phoenix project:

#### Research on Hex.pm
- Visit https://hex.pm and search for packages
- Check the package's download statistics (prefer packages with higher weekly downloads)
- Review the package version history for stability indicators
- Look for packages with semantic versioning practices

#### Evaluate Package Quality
- **Documentation**: Ensure comprehensive docs on hex.pm with examples
- **Maintenance**: Check for regular releases and bug fixes
- **Community**: Look for packages used by popular projects
- **OTP Compatibility**: Verify support for your Elixir/OTP versions

#### Check the Source Repository
- Visit the GitHub/GitLab repository linked from hex.pm
- Verify recent commits (within last 6 months for active packages)
- Review issue resolution patterns and response times
- Check CI/CD status and test coverage
- Look for clear contribution guidelines
</conditional-block>

<conditional-block context-check="read-documentation-of-libraries">
IF documentation exists in `.claude/libraries_docs` or `.claude/phoenix_guides/*/**`:
  READ: The following guidelines
ELSE:
  FETCH: Use the following process to gather library documentation

  1. **Search documentation for project dependencies**:
     ```bash
     # Search documentation for specific packages
     mix usage_rules.search_docs "search term" -p [name of library] -p [another library]
     ```

  2. **Sync individual library documentation**:
     ```bash
     # For each library, create individual documentation file
     mix usage_rules.sync libraries_docs/[name of library].md [name of library]
     ```
  THEN: Re-read this section to access the gathered documentation
ELSE:
  SKIP: Library documentation section not relevant to current task
</conditional-block>