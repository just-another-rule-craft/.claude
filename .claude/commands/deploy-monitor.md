# Phoenix Deployment & Monitoring

Comprehensive deployment strategies and monitoring setup for Phoenix applications.

**Usage**: `/project:deploy-monitor [deployment task or monitoring setup]`

**Arguments**:
- `$ARGUMENTS`: Specific deployment task (production, staging, monitoring, etc.)

## Workflow

Handle deployment and monitoring for: $ARGUMENTS

Follow this systematic deployment approach:

### Phase 1: Pre-Deployment Planning
Use "think hard" to plan the deployment strategy:
- What is the target environment (staging, production)?
- What are the infrastructure requirements?
- What are the rollback procedures?
- What monitoring is needed?
- What are the security considerations?

### Phase 2: Environment Preparation

#### Production Configuration
```elixir
# config/prod.exs
import Config

config :my_app, MyAppWeb.Endpoint,
  url: [host: "example.com", port: 443, scheme: "https"],
  cache_static_manifest: "priv/static/cache_manifest.json",
  server: true

config :my_app, MyApp.Repo,
  # Database configuration from environment
  url: System.get_env("DATABASE_URL"),
  pool_size: String.to_integer(System.get_env("POOL_SIZE") || "10"),
  socket_options: [:inet6]

# SSL configuration
config :my_app, MyAppWeb.Endpoint,
  https: [
    port: 443,
    cipher_suite: :strong,
    keyfile: System.get_env("SSL_KEY_PATH"),
    certfile: System.get_env("SSL_CERT_PATH")
  ]

# Security headers
config :my_app, MyAppWeb.Endpoint,
  force_ssl: [rewrite_on: [:x_forwarded_proto]]
```

#### Runtime Configuration
```elixir
# config/runtime.exs
import Config

if System.get_env("PHX_SERVER") do
  config :my_app, MyAppWeb.Endpoint, server: true
end

if config_env() == :prod do
  database_url =
    System.get_env("DATABASE_URL") ||
    raise """
    Environment variable DATABASE_URL is missing.
    """

  config :my_app, MyApp.Repo,
    url: database_url,
    pool_size: String.to_integer(System.get_env("POOL_SIZE") || "10"),
    socket_options: [:inet6]

  secret_key_base =
    System.get_env("SECRET_KEY_BASE") ||
    raise """
    Environment variable SECRET_KEY_BASE is missing.
    """

  config :my_app, MyAppWeb.Endpoint,
    http: [
      ip: {0, 0, 0, 0, 0, 0, 0, 0},
      port: String.to_integer(System.get_env("PORT") || "4000")
    ],
    secret_key_base: secret_key_base
end
```

### Phase 3: Containerization with Docker

#### Dockerfile
```dockerfile
# Build stage
FROM hexpm/elixir:1.18.1-erlang-27.2-debian-bookworm-20241202-slim AS build

# Install build dependencies
RUN apt-get update -y && apt-get install -y build-essential git \
    && apt-get clean && rm -f /var/lib/apt/lists/*_*

WORKDIR /app

# Install hex and rebar
RUN mix local.hex --force && \
    mix local.rebar --force

# Set build ENV
ENV MIX_ENV="prod"

# Copy mix files
COPY mix.exs mix.lock ./
RUN mix deps.get --only $MIX_ENV
RUN mkdir config

# Copy config files
COPY config/config.exs config/${MIX_ENV}.exs config/
RUN mix deps.compile

# Copy application code
COPY priv priv
COPY lib lib
COPY assets assets

# Compile assets
RUN mix assets.deploy

# Compile the release
RUN mix compile

# Changes to config/runtime.exs don't require recompiling the code
COPY config/runtime.exs config/

COPY rel rel
RUN mix release

# Runtime stage
FROM debian:bookworm-20241202-slim AS app

RUN apt-get update -y && \
  apt-get install -y libstdc++6 openssl libncurses5 locales ca-certificates \
  && apt-get clean && rm -f /var/lib/apt/lists/*_*

# Set the locale
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && locale-gen

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN useradd --create-home app
WORKDIR /app
RUN chown app:app /app
USER app:app

COPY --from=build --chown=app:app /app/_build/prod/rel/my_app ./

EXPOSE 4000
CMD ["bin/my_app", "start"]
```

#### Docker Compose for Development
```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "4000:4000"
    environment:
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/my_app_dev
      - SECRET_KEY_BASE=your_secret_key_base_here
      - PHX_HOST=localhost
    depends_on:
      - db
    volumes:
      - .:/app
      - /app/deps
      - /app/_build

  db:
    image: postgres:17
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=my_app_dev
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
```

### Phase 4: Release Management

#### Elixir Releases Configuration
```elixir
# rel/env.sh.eex
#!/bin/sh

# Database configuration
export DATABASE_URL="${DATABASE_URL}"
export POOL_SIZE="${POOL_SIZE:-10}"

# Application configuration
export SECRET_KEY_BASE="${SECRET_KEY_BASE}"
export PHX_HOST="${PHX_HOST}"
export PORT="${PORT:-4000}"

# SSL configuration
export SSL_KEY_PATH="${SSL_KEY_PATH}"
export SSL_CERT_PATH="${SSL_CERT_PATH}"
```

#### Release Commands
```bash
# Build release
MIX_ENV=prod mix release

# Run release
_build/prod/rel/my_app/bin/my_app start

# Run database migrations in production
_build/prod/rel/my_app/bin/my_app eval "MyApp.Release.migrate"

# Create custom release commands
_build/prod/rel/my_app/bin/my_app rpc "MyApp.Release.seed"
```

### Phase 5: Cloud Deployment

#### Kubernetes Deployment
```yaml
# k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:latest
        ports:
        - containerPort: 4000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: my-app-secrets
              key: database-url
        - name: SECRET_KEY_BASE
          valueFrom:
            secretKeyRef:
              name: my-app-secrets
              key: secret-key-base
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 4000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 4000
          initialDelaySeconds: 5
          periodSeconds: 5
```

#### Service and Ingress
```yaml
# k8s/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 4000
  type: ClusterIP

---
# k8s/ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
  - hosts:
    - example.com
    secretName: my-app-tls
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-app-service
            port:
              number: 80
```

### Phase 6: Monitoring and Observability

#### Phoenix LiveDashboard Setup
```elixir
# lib/my_app_web/router.ex
defmodule MyAppWeb.Router do
  import Phoenix.LiveDashboard.Router

  scope "/" do
    pipe_through :browser
    live_dashboard "/dashboard", metrics: MyAppWeb.Telemetry
  end
end
```

#### Telemetry Configuration
```elixir
# lib/my_app_web/telemetry.ex
defmodule MyAppWeb.Telemetry do
  use Supervisor
  import Telemetry.Metrics

  def start_link(arg) do
    Supervisor.start_link(__MODULE__, arg, name: __MODULE__)
  end

  def init(_arg) do
    children = [
      {:telemetry_poller, measurements: periodic_measurements(), period: 10_000},
      {TelemetryMetricsPrometheus, metrics: metrics()}
    ]

    Supervisor.init(children, strategy: :one_for_one)
  end

  def metrics do
    [
      # Phoenix Metrics
      summary("phoenix.endpoint.stop.duration",
        unit: {:native, :millisecond}
      ),
      summary("phoenix.router_dispatch.stop.duration",
        tags: [:route],
        unit: {:native, :millisecond}
      ),

      # Database Metrics
      summary("my_app.repo.query.total_time",
        unit: {:native, :millisecond}
      ),
      counter("my_app.repo.query.count"),

      # VM Metrics
      summary("vm.memory.total", unit: {:byte, :kilobyte}),
      summary("vm.total_run_queue_lengths.total"),
      summary("vm.total_run_queue_lengths.cpu"),
      summary("vm.total_run_queue_lengths.io"),

      # Custom Business Metrics
      counter("my_app.users.created"),
      counter("my_app.orders.completed"),
      distribution("my_app.orders.value", unit: :dollar)
    ]
  end

  defp periodic_measurements do
    [
      {MyApp, :measure_users, []},
      {MyApp, :measure_orders, []}
    ]
  end
end
```

#### Prometheus Integration
```elixir
# mix.exs
defp deps do
  [
    {:telemetry_metrics_prometheus, "~> 1.1"},
    {:telemetry_poller, "~> 1.0"}
  ]
end

# config/prod.exs
config :telemetry_metrics_prometheus,
  port: 9568,
  path: "/metrics"
```

#### Health Check Endpoints
```elixir
# lib/my_app_web/controllers/health_controller.ex
defmodule MyAppWeb.HealthController do
  use MyAppWeb, :controller

  def show(conn, _params) do
    # Basic health check
    json(conn, %{status: "ok", timestamp: DateTime.utc_now()})
  end

  def ready(conn, _params) do
    # Readiness check (database, external services)
    case check_dependencies() do
      :ok ->
        json(conn, %{status: "ready"})
      {:error, reason} ->
        conn
        |> put_status(503)
        |> json(%{status: "not_ready", reason: reason})
    end
  end

  defp check_dependencies do
    with :ok <- check_database(),
         :ok <- check_redis(),
         :ok <- check_external_apis() do
      :ok
    else
      error -> error
    end
  end
end
```

### Phase 7: Logging and Error Tracking

#### Structured Logging
```elixir
# config/prod.exs
config :logger,
  level: :info,
  backends: [:console, {LoggerFileBackend, :info}]

config :logger, :console,
  format: "$time $metadata[$level] $message\n",
  metadata: [:request_id, :user_id, :ip_address]
```

#### Error Tracking with Sentry
```elixir
# mix.exs
{:sentry, "~> 8.0"}

# config/prod.exs
config :sentry,
  dsn: System.get_env("SENTRY_DSN"),
  environment_name: :prod,
  enable_source_code_context: true,
  root_source_code_path: File.cwd!(),
  tags: %{
    env: "production"
  },
  included_environments: [:prod]

# lib/my_app_web/endpoint.ex
defmodule MyAppWeb.Endpoint do
  use Phoenix.Endpoint, otp_app: :my_app
  use Sentry.Phoenix.Endpoint
end
```

### Phase 8: Performance Monitoring

#### APM Integration
```elixir
# Application Performance Monitoring
config :my_app, :apm,
  service_name: "my-app",
  environment: "production"

# Custom telemetry events
def track_user_action(user_id, action) do
  :telemetry.execute([:my_app, :user, :action], %{count: 1}, %{
    user_id: user_id,
    action: action
  })
end
```

#### Database Query Monitoring
```elixir
# config/prod.exs
config :my_app, MyApp.Repo,
  log: false,  # Disable Ecto logging in favor of telemetry
  telemetry_prefix: [:my_app, :repo]

# Custom slow query detection
defmodule MyApp.SlowQueryTracker do
  def handle_event([:my_app, :repo, :query], measurements, metadata, _config) do
    if measurements.total_time > 1_000_000 do  # 1 second
      Logger.warn("Slow query detected",
        query: metadata.query,
        time: measurements.total_time
      )
    end
  end
end
```

### Phase 9: Deployment Automation

#### CI/CD Pipeline (GitHub Actions)
```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:17
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
    - uses: actions/checkout@v4
    - uses: erlef/setup-beam@v1
      with:
        elixir-version: '1.18.1'
        otp-version: '27.2'

    - name: Cache deps
      uses: actions/cache@v3
      with:
        path: deps
        key: ${{ runner.os }}-mix-${{ hashFiles('**/mix.lock') }}

    - name: Install dependencies
      run: mix deps.get

    - name: Run tests
      run: mix test
      env:
        DATABASE_URL: postgres://postgres:postgres@localhost/my_app_test

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
    - uses: actions/checkout@v4

    - name: Build and push Docker image
      run: |
        docker build -t my-app:latest .
        docker push ${{ secrets.DOCKER_REGISTRY }}/my-app:latest

    - name: Deploy to Kubernetes
      run: |
        kubectl set image deployment/my-app my-app=${{ secrets.DOCKER_REGISTRY }}/my-app:latest
```

### Phase 10: Maintenance and Updates

#### Blue-Green Deployment
```bash
# Deploy to green environment
kubectl apply -f k8s/green-deployment.yaml

# Test green environment
curl -f https://green.example.com/health

# Switch traffic to green
kubectl patch service my-app-service -p '{"spec":{"selector":{"version":"green"}}}'

# Monitor for issues
# If problems, switch back to blue quickly
```

#### Database Migration Strategy
```elixir
# Safe migration deployment
defmodule MyApp.Release do
  def migrate do
    for repo <- repos() do
      {:ok, _, _} = Ecto.Migrator.with_repo(repo, &Ecto.Migrator.run(&1, :up, all: true))
    end
  end

  def rollback(repo, version) do
    {:ok, _, _} = Ecto.Migrator.with_repo(repo, &Ecto.Migrator.run(&1, :down, to: version))
  end

  defp repos do
    Application.load(:my_app)
    Application.fetch_env!(:my_app, :ecto_repos)
  end
end
```

## Deployment Checklist

**Pre-Deployment**:
- [ ] All tests passing
- [ ] Security vulnerabilities addressed
- [ ] Performance benchmarks met
- [ ] Database migrations tested
- [ ] Rollback procedures documented

**Production Deployment**:
- [ ] Health checks configured
- [ ] Monitoring and alerting active
- [ ] Error tracking enabled
- [ ] Performance monitoring configured
- [ ] Backup procedures verified

**Post-Deployment**:
- [ ] Application health verified
- [ ] Performance metrics reviewed
- [ ] Error rates monitored
- [ ] User experience validated
- [ ] Documentation updated

Remember: Successful deployment is about reliability, monitoring, and quick recovery. Always plan for failures and have rollback procedures ready.
