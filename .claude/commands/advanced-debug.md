# Advanced Phoenix Debugging

Advanced debugging workflow for complex Phoenix issues using systematic investigation.

**Usage**: `/project:advanced-debug [issue description]`

**Arguments**:
- `$ARGUMENTS`: Detailed description of the complex issue

## Workflow

Debug the complex Phoenix issue: $ARGUMENTS

Follow this advanced debugging protocol:

### Phase 1: Evidence Collection (Think First)
Use "think hard" to understand the problem scope:
- What is the expected behavior?
- What is the actual behavior?
- When did this issue start occurring?
- What are the environmental factors?

### Phase 2: Systematic Investigation
**Log Analysis**:
```
Use `get_logs` to analyze recent errors with focus on:
- Stack traces and error patterns
- Request/response cycles involved
- Database query logs if applicable
- Performance metrics and timing
```

**Code Context Gathering**:
```
Use `get_source_location` to find:
- The entry point where the issue occurs
- Related modules and functions
- Dependencies and integrations involved

Use `get_docs` to understand:
- Expected function contracts
- API specifications
- Error handling patterns
```

**Data State Analysis**:
```
If database-related, use `get_ecto_schemas` then `execute_sql_query` to:
- Check data integrity
- Verify constraint violations
- Analyze relationship consistency
- Examine index usage
```

### Phase 3: Reproduction Strategy
**Isolated Testing**:
```
Use `project_eval` to:
- Create minimal reproduction cases
- Test individual functions in isolation
- Verify data transformations
- Check edge cases and boundary conditions
```

**Environment Testing**:
```
Test in different scenarios:
- Various user roles and permissions
- Different data states
- Concurrent access patterns
- Resource constraints
```

### Phase 4: Root Cause Analysis
Use "ultrathink" for complex system interactions:
- Analyze the complete request lifecycle
- Identify race conditions or timing issues
- Consider memory leaks or resource exhaustion
- Examine third-party service interactions

### Phase 5: Solution Implementation
**Defensive Programming**:
- Implement comprehensive error handling
- Add validation at system boundaries
- Use circuit breakers for external services
- Add monitoring and alerting

**Testing Strategy**:
- Write tests that reproduce the bug
- Add regression tests for edge cases
- Test error recovery scenarios
- Validate performance under load

### Phase 6: Prevention Measures
**Code Improvements**:
- Add static analysis with Dialyzer
- Implement property-based testing
- Add comprehensive logging
- Document known edge cases

**Process Improvements**:
- Update deployment procedures
- Add health checks and monitoring
- Document troubleshooting procedures
- Create runbooks for common issues

## Advanced Debugging Techniques

### Memory and Performance Issues
```
1. Use `project_eval` to check process memory usage
2. Analyze GenServer state accumulation
3. Check for message queue buildup
4. Profile with :observer or :fprof
```

### Distributed System Issues
```
1. Check node connectivity and clustering
2. Verify distributed state consistency
3. Analyze network partitioning scenarios
4. Test failover and recovery procedures
```

### Database Performance Issues
```
1. Use `execute_sql_query` with EXPLAIN ANALYZE
2. Check index usage and query plans
3. Analyze connection pool saturation
4. Monitor transaction lock contention
```

### LiveView State Issues
```
1. Examine mount/3 and handle_params/3 lifecycle
2. Check for state leaks in assigns
3. Verify event handling and state transitions
4. Test reconnection scenarios
```

## Emergency Procedures

### Production Debugging
- Never debug directly in production
- Use log aggregation and monitoring tools
- Create reproduction environments
- Implement feature flags for quick rollbacks

### Critical Issue Response
1. **Immediate**: Stop the bleeding (rollback, circuit breaker)
2. **Short-term**: Implement temporary workaround
3. **Long-term**: Root cause analysis and permanent fix
4. **Follow-up**: Post-mortem and prevention measures

## Documentation Requirements
- Document all findings in investigation log
- Update troubleshooting documentation
- Share learnings with team
- Update monitoring and alerting based on findings

Remember: Advanced debugging requires patience, systematic thinking, and comprehensive documentation. Don't rush to solutions without understanding the root cause.
