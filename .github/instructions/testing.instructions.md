---
applyTo: "test/**/*.exs"
---

# Testing Instructions

When working with test files:

- Use descriptive test names that explain the expected behavior
- Follow the Arrange-Act-Assert pattern in test structure
- Test both success and failure scenarios
- Use `ExUnit.Case` for unit tests and `Phoenix.ConnTest` for controller tests
- Create test factories for consistent test data
- Mock external dependencies appropriately
- Test edge cases and error conditions
- Use `Phoenix.LiveViewTest` for testing LiveView components
- Ensure proper test data cleanup between tests
- Write integration tests for complete user workflows
