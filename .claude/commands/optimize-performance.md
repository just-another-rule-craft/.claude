# Phoenix Performance Optimization

Systematic performance analysis and optimization for Phoenix applications.

**Usage**: `/project:optimize-performance [performance issue description]`

**Arguments**:
- `$ARGUMENTS`: Description of performance issue or area to optimize

## Workflow

Optimize Phoenix performance for: $ARGUMENTS

Follow this performance optimization methodology:

### Phase 1: Performance Baseline (Measure First)
Use "think" to establish current performance metrics:
- What are the current response times?
- Where are the bottlenecks occurring?
- What are the resource utilization patterns?

**Measurement Tools**:
```
Use `get_logs` to analyze:
- Request/response times
- Database query durations
- Memory usage patterns
- Error rates and timeouts

Use `project_eval` to benchmark:
- Function execution times
- Memory allocations
- Process message queue sizes
- Concurrent request handling
```

### Phase 2: Database Optimization
**Query Analysis**:
```
Use `execute_sql_query` with EXPLAIN ANALYZE to:
- Identify slow queries
- Check index usage
- Analyze join strategies
- Find sequential scans

Use `get_ecto_schemas` to:
- Review association patterns
- Check for N+1 query opportunities
- Verify data types and constraints
- Analyze schema relationships
```

**Common Database Optimizations**:
- Add database indexes for frequent queries
- Use Ecto.Repo.preload/2 to eliminate N+1 queries
- Implement database-level aggregations
- Consider read replicas for read-heavy workloads
- Use prepared statements for repeated queries

### Phase 3: Application Layer Optimization
**Context and Business Logic**:
```
Use `get_source_location` to find performance bottlenecks in:
- Context functions with heavy computation
- Data transformation pipelines
- External service integrations
- File processing operations
```

**Optimization Strategies**:
- Cache expensive computations with ETS or external cache
- Use GenServer for stateful operations
- Implement background job processing with Oban
- Stream large datasets instead of loading all at once
- Use Task.async for concurrent operations

### Phase 4: LiveView Performance
**LiveView Analysis**:
```
Use `list_liveview_pages` and `get_source_location` to optimize:
- Mount and handle_params functions
- Event handler performance
- Template rendering speed
- State management efficiency
```

**LiveView Optimizations**:
- Minimize assigns and use temporary assigns
- Implement efficient diff algorithms
- Use streams for large collections
- Optimize event handling patterns
- Consider component-based architecture

### Phase 5: Frontend and Asset Optimization
**Asset Pipeline**:
- Optimize TailwindCSS purging and minification
- Implement proper asset caching strategies
- Use CDN for static assets
- Minimize JavaScript bundle sizes
- Implement lazy loading for images and components

**Network Optimization**:
- Enable GZIP compression
- Implement proper HTTP caching headers
- Use HTTP/2 server push where appropriate
- Optimize Phoenix Channel usage
- Implement connection pooling

### Phase 6: System-Level Optimization
**BEAM VM Tuning**:
```
Use `project_eval` to check:
- Process count and memory usage
- Scheduler utilization
- Garbage collection patterns
- Memory fragmentation
```

**Production Optimizations**:
- Tune BEAM VM parameters
- Configure proper connection pools
- Implement health checks and circuit breakers
- Use clustered deployments for scalability
- Implement proper logging levels

### Phase 7: Monitoring and Alerting
**Performance Monitoring**:
- Set up Phoenix LiveDashboard
- Implement custom telemetry events
- Monitor database performance metrics
- Track user experience metrics
- Set up alerting for performance degradation

**Continuous Optimization**:
- Regular performance testing
- Automated performance regression detection
- Load testing with realistic scenarios
- Performance budgets and SLA monitoring

## Performance Testing Patterns

### Load Testing
```elixir
# Use project_eval to create load test scenarios
defmodule LoadTest do
  def simulate_user_load(concurrent_users, duration) do
    # Implementation for load testing
  end
end
```

### Benchmarking
```elixir
# Use project_eval to benchmark functions
:timer.tc(fn ->
  # Function to benchmark
end)
```

### Memory Profiling
```elixir
# Use project_eval to check memory usage
:observer_cli.start()
Process.info(self(), :memory)
```

## Common Performance Anti-Patterns

### Database Issues
- N+1 queries (use preload)
- Missing indexes (check EXPLAIN)
- Over-fetching data (select specific fields)
- Long-running transactions (keep them short)

### Application Issues
- Synchronous external API calls (use async)
- Heavy computations in request cycle (use background jobs)
- Large GenServer state (split or use ETS)
- Memory leaks in processes (monitor and restart)

### Frontend Issues
- Large DOM manipulations (use efficient LiveView updates)
- Unoptimized assets (proper bundling and compression)
- Too many HTTP requests (bundle and cache)
- Inefficient CSS (optimize Tailwind purging)

## Performance Optimization Checklist

**Database Layer**:
- [ ] Indexes added for all frequent queries
- [ ] N+1 queries eliminated with preloading
- [ ] Query performance analyzed with EXPLAIN
- [ ] Connection pool properly configured

**Application Layer**:
- [ ] Expensive operations moved to background jobs
- [ ] Caching implemented for repeated computations
- [ ] External API calls made asynchronous
- [ ] Process memory usage monitored

**Frontend Layer**:
- [ ] Assets properly minified and compressed
- [ ] HTTP caching headers implemented
- [ ] LiveView updates optimized
- [ ] Large collections use streaming

**Infrastructure**:
- [ ] Production deployment optimized
- [ ] Monitoring and alerting configured
- [ ] Load testing implemented
- [ ] Performance budgets established

Remember: Always measure before optimizing, and focus on the biggest bottlenecks first. Premature optimization is the root of all evil, but systematic performance improvement is essential for scalable applications.
