---
applyTo: "config/*.exs"
---

# Configuration Instructions

When working with configuration files:

- Use environment-specific configuration appropriately
- Keep sensitive data in environment variables, not in code
- Use `config/runtime.exs` for runtime configuration
- Document configuration options with clear comments
- Validate configuration values at application startup
- Use appropriate defaults for development environment
- Ensure production configuration is secure and optimized
- Use `Config` module functions for configuration management
- Group related configuration options logically
- Follow Phoenix configuration conventions and patterns
