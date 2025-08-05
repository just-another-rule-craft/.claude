# Code Style Guide

## Context

Global code style rules.

<conditional-block task-condition="html-css-tailwind" context-check="html-css-style">
IF current task involves writing or updating HTML, CSS, or TailwindCSS:
  IF html-style.md AND css-style.md already in context:
    SKIP: Re-reading these files
    NOTE: "Using HTML/CSS style guides already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get HTML formatting rules from code-style/html-style.md"
        REQUEST: "Get CSS and TailwindCSS rules from code-style/css-style.md"
        PROCESS: Returned style rules
      ELSE:
        READ the following style guides (only if not already in context):
        -#file:.claude/standards/code-style/html-style.md (if not in context)
        -#file:.claude/standards/code-style/css-style.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: HTML/CSS style guides not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-controllers" context-check="controllers-guide">
IF current task involves writing or updating Phoenix controllers:
  IF controllers.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix controllers guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix controller patterns from phoenix_guide/controllers.md"
        PROCESS: Returned controller guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/controllers.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix controllers guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-security" context-check="security-guide">
IF current task involves security, authentication, authorization, or handling untrusted input:
  IF security.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix security guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix security practices from phoenix_guide/security.md"
        PROCESS: Returned security guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/security.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix security guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-contexts" context-check="contexts-guide">
IF current task involves Phoenix contexts, data modeling, or database operations:
  IF contexts.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix contexts guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix contexts patterns from phoenix_guide/data_modelling/contexts.md"
        PROCESS: Returned contexts guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/data_modelling/contexts.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix contexts guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-deployment" context-check="deployment-guide">
IF current task involves deployment, hosting, or production configuration:
  IF deployment.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix deployment guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix deployment practices from phoenix_guide/deployment/deployment.md"
        PROCESS: Returned deployment guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/deployment/deployment.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix deployment guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-assets" context-check="assets-guide">
IF current task involves asset management, CSS, JavaScript, or static files:
  IF asset_management.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix asset management guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix asset management from phoenix_guide/asset_management.md"
        PROCESS: Returned asset management guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/asset_management.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix asset management guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-telemetry" context-check="telemetry-guide">
IF current task involves telemetry, metrics, monitoring, or performance tracking:
  IF telemetry.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix telemetry guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix telemetry patterns from phoenix_guide/telemetry.md"
        PROCESS: Returned telemetry guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/telemetry.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix telemetry guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-howto" context-check="howto-guides">
IF current task involves SSL, file uploads, custom error pages, or database swapping:
  IF relevant howto guides already in context:
    SKIP: Re-reading these files
    NOTE: "Using Phoenix howto guides already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get relevant Phoenix howto guides from phoenix_guide/howto/"
        PROCESS: Returned howto guides
      ELSE:
        READ the following guides as needed (only if not already in context):
        -#file:.claude/phoenix_guide/howto/using_ssl.md (for SSL tasks)
        -#file:.claude/phoenix_guide/howto/file_uploads.md (for file upload tasks)
        -#file:.claude/phoenix_guide/howto/custom_error_pages.md (for error page tasks)
        -#file:.claude/phoenix_guide/howto/swapping_databases.md (for database tasks)
        -#file:.claude/phoenix_guide/howto/writing_a_channels_client.md (for channels tasks)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix howto guides not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-router" context-check="router-cheatsheet">
IF current task involves Phoenix routing, URL patterns, or route helpers:
  IF router.cheatmd already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix router cheatsheet already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix router patterns from phoenix_guide/cheatsheets/router.cheatmd"
        PROCESS: Returned router cheatsheet
      ELSE:
        READ the following cheatsheet (only if not already in context):
        -#file:.claude/phoenix_guide/cheatsheets/router.cheatmd (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix router cheatsheet not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-directory-structure" context-check="structure-guide">
IF current task involves project organization, file structure, or code architecture:
  IF directory_structure.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix directory structure guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix directory structure from phoenix_guide/directory_structure.md"
        PROCESS: Returned structure guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/directory_structure.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix directory structure guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-first-context" context-check="first-context-guide">
IF current task involves creating your first Phoenix context or understanding context basics:
  IF your_first_context.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix first context guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix first context tutorial from phoenix_guide/data_modelling/your_first_context.md"
        PROCESS: Returned first context guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/data_modelling/your_first_context.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix first context guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-context-relationships" context-check="context-relationships-guide">
IF current task involves relationships within contexts or data associations:
  IF in_context_relationships.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix context relationships guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix context relationships from phoenix_guide/data_modelling/in_context_relationships.md"
        PROCESS: Returned context relationships guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/data_modelling/in_context_relationships.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix context relationships guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-cross-context" context-check="cross-context-guide">
IF current task involves cross-context boundaries or inter-context communication:
  IF cross_context_boundaries.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix cross-context boundaries guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix cross-context patterns from phoenix_guide/data_modelling/cross_context_boundaries.md"
        PROCESS: Returned cross-context guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/data_modelling/cross_context_boundaries.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix cross-context boundaries guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-data-modeling-examples" context-check="data-modeling-examples">
IF current task involves data modeling examples or advanced patterns:
  IF more_examples.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix data modeling examples already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix data modeling examples from phoenix_guide/data_modelling/more_examples.md"
        PROCESS: Returned data modeling examples
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/data_modelling/more_examples.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix data modeling examples not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-data-modeling-faq" context-check="data-modeling-faq">
IF current task involves troubleshooting data modeling or common questions:
  IF faq.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix data modeling FAQ already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix data modeling FAQ from phoenix_guide/data_modelling/faq.md"
        PROCESS: Returned data modeling FAQ
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/data_modelling/faq.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix data modeling FAQ not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-heroku-deployment" context-check="heroku-deployment-guide">
IF current task involves Heroku deployment or Heroku-specific configuration:
  IF heroku.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix Heroku deployment guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix Heroku deployment from phoenix_guide/deployment/heroku.md"
        PROCESS: Returned Heroku deployment guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/deployment/heroku.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix Heroku deployment guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-gigalixir-deployment" context-check="gigalixir-deployment-guide">
IF current task involves Gigalixir deployment or Gigalixir-specific configuration:
  IF gigalixir.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix Gigalixir deployment guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix Gigalixir deployment from phoenix_guide/deployment/gigalixir.md"
        PROCESS: Returned Gigalixir deployment guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/deployment/gigalixir.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix Gigalixir deployment guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-ssl-setup" context-check="ssl-setup-guide">
IF current task involves SSL setup, HTTPS configuration, or TLS certificates:
  IF using_ssl.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix SSL setup guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix SSL setup from phoenix_guide/howto/using_ssl.md"
        PROCESS: Returned SSL setup guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/howto/using_ssl.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix SSL setup guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-file-uploads" context-check="file-uploads-guide">
IF current task involves file uploads, multipart forms, or file handling:
  IF file_uploads.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix file uploads guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix file uploads from phoenix_guide/howto/file_uploads.md"
        PROCESS: Returned file uploads guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/howto/file_uploads.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix file uploads guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-custom-error-pages" context-check="custom-error-pages-guide">
IF current task involves custom error pages, error handling, or error templates:
  IF custom_error_pages.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix custom error pages guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix custom error pages from phoenix_guide/howto/custom_error_pages.md"
        PROCESS: Returned custom error pages guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/howto/custom_error_pages.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix custom error pages guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-database-swapping" context-check="database-swapping-guide">
IF current task involves database swapping, database migration, or changing database adapters:
  IF swapping_databases.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix database swapping guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix database swapping from phoenix_guide/howto/swapping_databases.md"
        PROCESS: Returned database swapping guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/howto/swapping_databases.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix database swapping guide not relevant to current task
</conditional-block>

<conditional-block task-condition="phoenix-channels-client" context-check="channels-client-guide">
IF current task involves writing Phoenix channels clients or WebSocket clients:
  IF writing_a_channels_client.md already in context:
    SKIP: Re-reading this file
    NOTE: "Using Phoenix channels client guide already in context"
  ELSE:
    <context_fetcher_strategy>
      IF current agent is Claude Code AND context-fetcher agent exists:
        USE: @agent:context-fetcher
        REQUEST: "Get Phoenix channels client from phoenix_guide/howto/writing_a_channels_client.md"
        PROCESS: Returned channels client guide
      ELSE:
        READ the following guide (only if not already in context):
        -#file:.claude/phoenix_guide/howto/writing_a_channels_client.md (if not in context)
    </context_fetcher_strategy>
ELSE:
  SKIP: Phoenix channels client guide not relevant to current task
</conditional-block>
