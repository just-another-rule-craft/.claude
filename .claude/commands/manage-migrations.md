# Phoenix Migration Management

Comprehensive database migration planning, review, and deployment for Phoenix applications.

**Usage**: `/project:manage-migrations [migration task]`

**Arguments**:
- `$ARGUMENTS`: Specific migration task (create, review, deploy, rollback, etc.)

## Workflow

Manage database migrations for: $ARGUMENTS

Follow this migration management protocol:

### Phase 1: Migration Planning (Think First)
Use "think hard" to plan database changes:
- What is the business requirement driving this change?
- How will this affect existing data?
- What are the rollback requirements?
- Are there any breaking changes?
- What is the deployment strategy?

### Phase 2: Schema Analysis
**Current State Assessment**:
```
Use `get_ecto_schemas` to understand:
- Current schema definitions
- Existing associations and constraints
- Data types and validations
- Index requirements

Use `execute_sql_query` to examine:
- Current data distribution
- Constraint violations that might prevent migration
- Index usage and performance characteristics
- Foreign key relationships
```

### Phase 3: Migration Design Patterns

#### Safe Migration Patterns
**Adding Columns**:
```elixir
# Safe: Adding nullable columns
alter table(:users) do
  add :email_verified_at, :utc_datetime
end

# Safe: Adding columns with defaults
alter table(:users) do
  add :status, :string, default: "active"
end
```

**Creating Indexes**:
```elixir
# Safe: Creating indexes concurrently (PostgreSQL)
create index(:users, [:email], concurrently: true)

# Safe: Partial indexes for performance
create index(:users, [:status], where: "status = 'active'")
```

**Adding Foreign Keys**:
```elixir
# Safe: Adding foreign keys with validation
alter table(:posts) do
  add :user_id, references(:users, on_delete: :delete_all)
end

create index(:posts, [:user_id])
```

#### Dangerous Migration Patterns (Avoid)
**Dropping Columns**:
```elixir
# DANGEROUS: Can cause application errors
alter table(:users) do
  remove :old_column
end

# BETTER: Use multi-step approach
# 1. Stop using column in code
# 2. Deploy application
# 3. Drop column in separate migration
```

**Changing Column Types**:
```elixir
# DANGEROUS: Can cause data loss
alter table(:users) do
  modify :score, :string  # from :integer
end

# BETTER: Create new column, migrate data, drop old
```

**Adding NOT NULL Constraints**:
```elixir
# DANGEROUS: Can fail if existing NULL values
alter table(:users) do
  modify :email, :string, null: false
end

# BETTER: Update NULLs first, then add constraint
```

### Phase 4: Migration Implementation

#### Step-by-Step Migration Creation
```
1. Use `mix ecto.gen.migration descriptive_migration_name`
2. Use `get_source_location` to find generated migration file
3. Implement migration following safe patterns
4. Add corresponding rollback logic
5. Test migration on development data
```

#### Migration Safety Checklist
```
Use `project_eval` to verify:
- Migration is reversible (proper down/0 function)
- No data loss potential
- Performance impact is acceptable
- Foreign key constraints are properly handled
- Indexes are created for new foreign keys
```

### Phase 5: Migration Testing Strategy

#### Development Testing
```
Test migration safety:
1. Create realistic test data
2. Run migration: `mix ecto.migrate`
3. Verify data integrity
4. Test rollback: `mix ecto.rollback`
5. Verify rollback success
6. Re-run migration to test idempotency
```

#### Staging Environment Testing
```
Production-like testing:
1. Use production data subset
2. Measure migration execution time
3. Check application compatibility during migration
4. Verify zero-downtime deployment compatibility
5. Test emergency rollback procedures
```

### Phase 6: Production Deployment Strategy

#### Zero-Downtime Migrations
**Multi-Step Approach**:
```
Step 1: Additive changes only
- Add new columns (nullable)
- Add new indexes (concurrently)
- Add new tables

Step 2: Code deployment
- Deploy application code using both old and new schema
- Ensure backward compatibility

Step 3: Data migration
- Migrate data in background jobs
- Use batching for large datasets
- Monitor progress and performance

Step 4: Cleanup
- Remove old columns/tables
- Remove compatibility code
- Update documentation
```

#### Large Table Migrations
```
For tables with millions of rows:
1. Use online schema change tools (gh-ost, pt-online-schema-change)
2. Implement chunked data migration
3. Monitor replication lag
4. Plan for rollback scenarios
```

### Phase 7: Migration Monitoring

#### Progress Tracking
```
Use `get_logs` and `project_eval` to monitor:
- Migration execution progress
- Database performance impact
- Application error rates during migration
- Replication lag (if applicable)
```

#### Performance Monitoring
```
Monitor during migration:
- Database CPU and memory usage
- Connection pool utilization
- Query performance impact
- Disk space usage
```

### Phase 8: Common Migration Scenarios

#### User Schema Evolution
```elixir
# Adding user preferences
defmodule MyApp.Repo.Migrations.AddUserPreferences do
  use Ecto.Migration

  def up do
    alter table(:users) do
      add :preferences, :map, default: %{}
    end
  end

  def down do
    alter table(:users) do
      remove :preferences
    end
  end
end
```

#### Relationship Changes
```elixir
# Converting one-to-one to one-to-many
defmodule MyApp.Repo.Migrations.UserMultipleProfiles do
  use Ecto.Migration

  def up do
    create table(:user_profiles) do
      add :user_id, references(:users, on_delete: :delete_all)
      add :profile_type, :string
      add :data, :map
      timestamps()
    end

    create index(:user_profiles, [:user_id])
    create unique_index(:user_profiles, [:user_id, :profile_type])
  end

  def down do
    drop table(:user_profiles)
  end
end
```

#### Data Cleanup Migrations
```elixir
# Removing orphaned records
defmodule MyApp.Repo.Migrations.CleanupOrphanedPosts do
  use Ecto.Migration
  import Ecto.Query

  def up do
    repo().delete_all(
      from p in "posts",
      left_join: u in "users", on: p.user_id == u.id,
      where: is_nil(u.id)
    )
  end

  def down do
    # Cannot restore deleted data
    :ok
  end
end
```

## Migration Security Considerations

### Data Protection
- Never expose sensitive data in migration logs
- Use encrypted columns for PII
- Implement proper access controls
- Audit migration execution

### Backup Strategy
- Always backup before major migrations
- Test backup restoration procedures
- Document recovery processes
- Plan for point-in-time recovery

## Emergency Procedures

### Migration Failure Recovery
```
1. Stop application deployment
2. Assess the failure impact
3. Execute rollback if safe
4. Fix data inconsistencies
5. Plan corrective migration
6. Document incident and lessons learned
```

### Production Hotfixes
```
For critical production issues:
1. Create emergency migration branch
2. Implement minimal fix migration
3. Test in staging with production data
4. Deploy with monitoring
5. Follow up with comprehensive fix
```

## Migration Documentation Requirements

### Migration Comments
```elixir
defmodule MyApp.Repo.Migrations.AddUserEmailVerification do
  use Ecto.Migration

  # Purpose: Add email verification tracking for security compliance
  # Risk: Low - Adding nullable column with default
  # Rollback: Safe - Column can be removed without data loss
  # Estimated time: < 1 minute for tables up to 1M rows

  def up do
    alter table(:users) do
      add :email_verified_at, :utc_datetime
      add :email_verification_token, :string
    end

    create index(:users, [:email_verification_token])
  end

  def down do
    alter table(:users) do
      remove :email_verified_at
      remove :email_verification_token
    end
  end
end
```

### Migration Log
- Document migration purpose and business context
- Record execution time and any issues
- Note data changes and impact
- Update schema documentation
- Record rollback procedures and testing

Remember: Database migrations are permanent changes that affect production data. Always prioritize safety, reversibility, and thorough testing over speed of delivery.
