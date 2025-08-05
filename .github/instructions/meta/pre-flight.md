---
description: Common Pre-Flight Steps for Phoenix Rules Instructions
applyTo: "**"
version: 1.1
encoding: UTF-8
---

# Pre-Flight Validation Rules

## Overview

These pre-flight checks ensure that all necessary prerequisites are met before executing complex development workflows in the Phoenix Rules framework.

## Validation Steps

1. **Agent Availability**: Verify that required specialized agents are available
   - For steps specifying `subagent=""` attribute, ensure the agent exists and is functional
   - Validate agent capabilities match the requested operation

2. **Context Requirements**: Ensure sufficient context is available
   - Check if required documentation files are accessible
   - Verify that project structure matches expected patterns

3. **Environment Validation**: Confirm development environment is properly configured
   - Elixir and Phoenix versions meet requirements
   - Database connectivity (if required)
   - Asset compilation tools availability

4. **Sequential Processing**: Process all XML blocks in the defined order
   - Respect dependencies between workflow steps
   - Maintain state consistency across operations

## Best Practices

- Always validate prerequisites before starting complex workflows
- Use exact templates as provided in the instruction files
- Log validation results for debugging and audit purposes
- Fail fast if critical prerequisites are not met
