---
applyTo: "priv/repo/migrations/*.exs"
---

# Database Migration Instructions

When working with database migrations:

- Use descriptive migration names that explain the change
- Include both `up` and `down` functions for reversibility
- Add proper indexes for foreign keys and frequently queried columns
- Use appropriate data types for each column
- Include constraints and validations at the database level
- Add comments to complex migrations
- Test migrations with sample data before deployment
- Consider performance impact of migrations on large datasets
- Use transactions for complex multi-step migrations
- Follow Ecto migration best practices and conventions
