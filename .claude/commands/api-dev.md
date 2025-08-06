# Phoenix API Development & Documentation

Comprehensive guide for building, documenting, and maintaining APIs in Phoenix applications.

**Usage**: `/project:api-dev [api task or component]`

**Arguments**:
- `$ARGUMENTS`: Specific API task (REST, GraphQL, documentation, versioning, etc.)

## Workflow

Handle API development for: $ARGUMENTS

Follow this systematic API development approach:

### Phase 1: API Architecture Planning
Use "think hard" to plan the API strategy:
- What type of API (REST, GraphQL, JSON:API)?
- What are the authentication requirements?
- What are the rate limiting needs?
- What are the versioning strategies?
- What documentation format is needed?

### Phase 2: REST API Development

#### API Router Setup
```elixir
# lib/my_app_web/router.ex
defmodule MyAppWeb.Router do
  use MyAppWeb, :router

  pipeline :api do
    plug :accepts, ["json"]
    plug :fetch_session
    plug :protect_from_forgery
    plug :put_secure_browser_headers
  end

  pipeline :api_auth do
    plug :accepts, ["json"]
    plug MyAppWeb.Plugs.ApiAuth
    plug MyAppWeb.Plugs.RateLimit
  end

  scope "/api/v1", MyAppWeb.Api.V1, as: :api_v1 do
    pipe_through :api

    # Public endpoints
    post "/auth/login", AuthController, :login
    post "/users/register", UserController, :register
    get "/health", HealthController, :show

    # Protected endpoints
    pipe_through :api_auth

    resources "/users", UserController, except: [:new, :edit]
    resources "/posts", PostController, except: [:new, :edit] do
      resources "/comments", CommentController, except: [:new, :edit]
    end

    # Custom routes
    get "/users/:id/posts", UserController, :posts
    post "/posts/:id/like", PostController, :like
    delete "/posts/:id/like", PostController, :unlike
  end

  scope "/api/v2", MyAppWeb.Api.V2, as: :api_v2 do
    pipe_through [:api, :api_auth]

    # V2 endpoints with different structure
    resources "/profiles", ProfileController
    resources "/articles", ArticleController
  end
end
```

#### Base API Controller
```elixir
# lib/my_app_web/controllers/api/base_controller.ex
defmodule MyAppWeb.Api.BaseController do
  defmacro __using__(_opts) do
    quote do
      use MyAppWeb, :controller
      import MyAppWeb.Api.BaseController

      action_fallback MyAppWeb.Api.FallbackController
    end
  end

  # Standard JSON response helpers
  def render_success(conn, data, status \\ 200) do
    conn
    |> put_status(status)
    |> json(%{
      success: true,
      data: data,
      timestamp: DateTime.utc_now()
    })
  end

  def render_error(conn, message, status \\ 400, details \\ %{}) do
    conn
    |> put_status(status)
    |> json(%{
      success: false,
      error: %{
        message: message,
        details: details
      },
      timestamp: DateTime.utc_now()
    })
  end

  def render_validation_errors(conn, changeset) do
    errors = Ecto.Changeset.traverse_errors(changeset, fn {msg, opts} ->
      Enum.reduce(opts, msg, fn {key, value}, acc ->
        String.replace(acc, "%{#{key}}", to_string(value))
      end)
    end)

    render_error(conn, "Validation failed", 422, errors)
  end
end
```

#### API Controller Example
```elixir
# lib/my_app_web/controllers/api/v1/user_controller.ex
defmodule MyAppWeb.Api.V1.UserController do
  use MyAppWeb.Api.BaseController

  alias MyApp.Accounts
  alias MyApp.Accounts.User

  def index(conn, params) do
    users = Accounts.list_users(params)
    render_success(conn, %{
      users: render_many(users, MyAppWeb.Api.V1.UserView, "user.json"),
      pagination: build_pagination(users, params)
    })
  end

  def show(conn, %{"id" => id}) do
    with {:ok, user} <- Accounts.get_user(id) do
      render_success(conn, %{
        user: render_one(user, MyAppWeb.Api.V1.UserView, "user.json")
      })
    end
  end

  def create(conn, %{"user" => user_params}) do
    with {:ok, user} <- Accounts.create_user(user_params) do
      conn
      |> put_status(:created)
      |> render_success(%{
        user: render_one(user, MyAppWeb.Api.V1.UserView, "user.json")
      }, 201)
    end
  end

  def update(conn, %{"id" => id, "user" => user_params}) do
    with {:ok, user} <- Accounts.get_user(id),
         {:ok, user} <- Accounts.update_user(user, user_params) do
      render_success(conn, %{
        user: render_one(user, MyAppWeb.Api.V1.UserView, "user.json")
      })
    end
  end

  def delete(conn, %{"id" => id}) do
    with {:ok, user} <- Accounts.get_user(id),
         {:ok, _user} <- Accounts.delete_user(user) do
      render_success(conn, %{message: "User deleted successfully"})
    end
  end

  def posts(conn, %{"id" => id} = params) do
    with {:ok, user} <- Accounts.get_user(id) do
      posts = Accounts.list_user_posts(user, params)
      render_success(conn, %{
        posts: render_many(posts, MyAppWeb.Api.V1.PostView, "post.json"),
        pagination: build_pagination(posts, params)
      })
    end
  end

  defp build_pagination(data, params) do
    %{
      page: Map.get(params, "page", 1),
      per_page: Map.get(params, "per_page", 20),
      total: length(data),
      has_next: false,  # Calculate based on actual pagination logic
      has_prev: false
    }
  end
end
```

#### API Views and Serialization
```elixir
# lib/my_app_web/views/api/v1/user_view.ex
defmodule MyAppWeb.Api.V1.UserView do
  use MyAppWeb, :view

  def render("user.json", %{user: user}) do
    %{
      id: user.id,
      email: user.email,
      name: user.name,
      avatar_url: user.avatar_url,
      created_at: user.inserted_at,
      updated_at: user.updated_at,
      profile: render_profile(user.profile)
    }
  end

  def render("user_summary.json", %{user: user}) do
    %{
      id: user.id,
      name: user.name,
      avatar_url: user.avatar_url
    }
  end

  defp render_profile(nil), do: nil
  defp render_profile(profile) do
    %{
      bio: profile.bio,
      location: profile.location,
      website: profile.website
    }
  end
end
```

#### Error Handling
```elixir
# lib/my_app_web/controllers/api/fallback_controller.ex
defmodule MyAppWeb.Api.FallbackController do
  use MyAppWeb.Api.BaseController

  def call(conn, {:error, %Ecto.Changeset{} = changeset}) do
    render_validation_errors(conn, changeset)
  end

  def call(conn, {:error, :not_found}) do
    render_error(conn, "Resource not found", 404)
  end

  def call(conn, {:error, :unauthorized}) do
    render_error(conn, "Unauthorized", 401)
  end

  def call(conn, {:error, :forbidden}) do
    render_error(conn, "Forbidden", 403)
  end

  def call(conn, {:error, message}) when is_binary(message) do
    render_error(conn, message, 400)
  end

  def call(conn, _error) do
    render_error(conn, "Internal server error", 500)
  end
end
```

### Phase 3: Authentication and Authorization

#### JWT Authentication
```elixir
# lib/my_app_web/plugs/api_auth.ex
defmodule MyAppWeb.Plugs.ApiAuth do
  import Plug.Conn
  import Phoenix.Controller

  alias MyApp.Accounts

  def init(opts), do: opts

  def call(conn, _opts) do
    case get_req_header(conn, "authorization") do
      ["Bearer " <> token] ->
        verify_token(conn, token)
      _ ->
        unauthorized(conn)
    end
  end

  defp verify_token(conn, token) do
    case MyApp.Token.verify_and_validate(token) do
      {:ok, %{"sub" => user_id}} ->
        case Accounts.get_user(user_id) do
          {:ok, user} ->
            assign(conn, :current_user, user)
          {:error, :not_found} ->
            unauthorized(conn)
        end
      {:error, _reason} ->
        unauthorized(conn)
    end
  end

  defp unauthorized(conn) do
    conn
    |> put_status(:unauthorized)
    |> json(%{error: "Unauthorized"})
    |> halt()
  end
end

# lib/my_app/token.ex
defmodule MyApp.Token do
  use Joken.Config

  @impl true
  def token_config do
    default_claims(
      default_exp: 24 * 60 * 60,  # 24 hours
      iss: "MyApp",
      aud: "MyApp:Users"
    )
  end

  def generate_and_sign(user) do
    extra_claims = %{
      "sub" => user.id,
      "email" => user.email,
      "role" => user.role
    }

    generate_and_sign!(extra_claims)
  end
end
```

#### Rate Limiting
```elixir
# lib/my_app_web/plugs/rate_limit.ex
defmodule MyAppWeb.Plugs.RateLimit do
  import Plug.Conn
  import Phoenix.Controller

  def init(opts) do
    Keyword.merge([
      max_requests: 100,
      window_ms: 60_000,  # 1 minute
      storage: :ets
    ], opts)
  end

  def call(conn, opts) do
    identifier = get_identifier(conn)

    case check_rate_limit(identifier, opts) do
      :ok ->
        conn
      {:error, :rate_limited} ->
        rate_limited(conn)
    end
  end

  defp get_identifier(conn) do
    case get_current_user(conn) do
      nil -> get_peer_data(conn).address |> :inet.ntoa() |> to_string()
      user -> "user:#{user.id}"
    end
  end

  defp check_rate_limit(identifier, opts) do
    # Implementation using Hammer, ExRated, or custom solution
    case Hammer.check_rate("api:#{identifier}", opts[:window_ms], opts[:max_requests]) do
      {:allow, _count} -> :ok
      {:deny, _limit} -> {:error, :rate_limited}
    end
  end

  defp rate_limited(conn) do
    conn
    |> put_status(:too_many_requests)
    |> json(%{error: "Rate limit exceeded"})
    |> halt()
  end

  defp get_current_user(conn), do: conn.assigns[:current_user]
end
```

### Phase 4: GraphQL API with Absinthe

#### GraphQL Schema
```elixir
# lib/my_app_web/schema.ex
defmodule MyAppWeb.Schema do
  use Absinthe.Schema

  alias MyApp.Accounts
  alias MyApp.Blog

  import_types MyAppWeb.Schema.{UserTypes, PostTypes}

  query do
    @desc "Get all users"
    field :users, list_of(:user) do
      arg :limit, :integer, default_value: 20
      arg :offset, :integer, default_value: 0
      resolve &Accounts.list_users/2
    end

    @desc "Get a user by ID"
    field :user, :user do
      arg :id, non_null(:id)
      resolve &Accounts.get_user/2
    end

    @desc "Get all posts"
    field :posts, list_of(:post) do
      arg :limit, :integer, default_value: 20
      arg :offset, :integer, default_value: 0
      resolve &Blog.list_posts/2
    end
  end

  mutation do
    @desc "Create a new user"
    field :create_user, :user do
      arg :input, non_null(:user_input)
      resolve &Accounts.create_user/2
    end

    @desc "Update a user"
    field :update_user, :user do
      arg :id, non_null(:id)
      arg :input, non_null(:user_input)
      resolve &Accounts.update_user/2
    end

    @desc "Create a new post"
    field :create_post, :post do
      arg :input, non_null(:post_input)
      resolve &Blog.create_post/2
    end
  end

  subscription do
    @desc "Subscribe to new posts"
    field :post_created, :post do
      config fn _args, %{context: %{current_user: user}} ->
        {:ok, topic: "posts:#{user.id}"}
      end
    end
  end
end

# lib/my_app_web/schema/user_types.ex
defmodule MyAppWeb.Schema.UserTypes do
  use Absinthe.Schema.Notation

  object :user do
    field :id, non_null(:id)
    field :email, non_null(:string)
    field :name, :string
    field :avatar_url, :string
    field :created_at, non_null(:datetime)
    field :updated_at, non_null(:datetime)

    field :posts, list_of(:post) do
      resolve fn user, _args, _resolution ->
        {:ok, MyApp.Blog.list_user_posts(user)}
      end
    end
  end

  input_object :user_input do
    field :email, non_null(:string)
    field :name, :string
    field :password, non_null(:string)
  end
end
```

#### GraphQL Context
```elixir
# lib/my_app_web/context.ex
defmodule MyAppWeb.Context do
  @behaviour Plug

  import Plug.Conn

  def init(opts), do: opts

  def call(conn, _) do
    context = build_context(conn)
    Absinthe.Plug.put_options(conn, context: context)
  end

  defp build_context(conn) do
    %{
      current_user: conn.assigns[:current_user],
      loader: Dataloader.new() |> add_loaders()
    }
  end

  defp add_loaders(loader) do
    loader
    |> Dataloader.add_source(Accounts, MyApp.Accounts.data())
    |> Dataloader.add_source(Blog, MyApp.Blog.data())
  end
end
```

### Phase 5: API Documentation

#### OpenAPI/Swagger Documentation
```elixir
# mix.exs
defp deps do
  [
    {:phoenix_swagger, "~> 0.8"},
    {:jason, "~> 1.0"}
  ]
end

# lib/my_app_web/controllers/api/v1/user_controller.ex
defmodule MyAppWeb.Api.V1.UserController do
  use MyAppWeb.Api.BaseController
  use PhoenixSwagger

  swagger_path :index do
    get "/api/v1/users"
    summary "List users"
    description "Returns a list of users with pagination"
    produces "application/json"

    parameter :page, :query, :integer, "Page number", default: 1
    parameter :per_page, :query, :integer, "Items per page", default: 20

    response 200, "Success", Schema.ref(:UsersResponse)
    response 401, "Unauthorized"
  end

  swagger_path :show do
    get "/api/v1/users/{id}"
    summary "Get user by ID"
    description "Returns a single user"
    produces "application/json"

    parameter :id, :path, :integer, "User ID", required: true

    response 200, "Success", Schema.ref(:UserResponse)
    response 404, "User not found"
    response 401, "Unauthorized"
  end

  swagger_path :create do
    post "/api/v1/users"
    summary "Create a new user"
    description "Creates a new user account"
    consumes "application/json"
    produces "application/json"

    parameter :user, :body, Schema.ref(:UserInput), "User parameters"

    response 201, "User created", Schema.ref(:UserResponse)
    response 422, "Validation errors"
  end

  def swagger_definitions do
    %{
      User: swagger_schema do
        title "User"
        description "A user of the application"
        properties do
          id :integer, "User ID"
          email :string, "Email address", format: :email
          name :string, "Full name"
          avatar_url :string, "Avatar image URL"
          created_at :string, "Creation timestamp", format: :datetime
          updated_at :string, "Last update timestamp", format: :datetime
        end
        required [:id, :email, :created_at, :updated_at]
      end,

      UserInput: swagger_schema do
        title "User Input"
        description "User creation/update parameters"
        properties do
          email :string, "Email address", format: :email
          name :string, "Full name"
          password :string, "Password", minLength: 6
        end
        required [:email, :password]
      end,

      UserResponse: swagger_schema do
        title "User Response"
        properties do
          success :boolean, "Request success status"
          data :object, "Response data" do
            properties do
              user Schema.ref(:User), "User object"
            end
          end
          timestamp :string, "Response timestamp", format: :datetime
        end
      end,

      UsersResponse: swagger_schema do
        title "Users Response"
        properties do
          success :boolean, "Request success status"
          data :object, "Response data" do
            properties do
              users type: :array, items: Schema.ref(:User), description: "List of users"
              pagination :object, "Pagination info" do
                properties do
                  page :integer, "Current page"
                  per_page :integer, "Items per page"
                  total :integer, "Total items"
                  has_next :boolean, "Has next page"
                  has_prev :boolean, "Has previous page"
                end
              end
            end
          end
          timestamp :string, "Response timestamp", format: :datetime
        end
      end
    }
  end
end
```

#### API Documentation Generation
```elixir
# config/config.exs
config :my_app, :phoenix_swagger,
  swagger_files: %{
    "priv/static/swagger.json" => [
      router: MyAppWeb.Router,
      endpoint: MyAppWeb.Endpoint
    ]
  }

# lib/my_app_web/router.ex
scope "/api/swagger" do
  forward "/", PhoenixSwagger.Plug.SwaggerUI,
    otp_app: :my_app,
    swagger_file: "swagger.json"
end
```

### Phase 6: API Testing

#### Controller Tests
```elixir
# test/my_app_web/controllers/api/v1/user_controller_test.exs
defmodule MyAppWeb.Api.V1.UserControllerTest do
  use MyAppWeb.ConnCase, async: true

  import MyApp.AccountsFixtures

  @create_attrs %{
    email: "test@example.com",
    name: "Test User",
    password: "password123"
  }

  @update_attrs %{
    name: "Updated Name"
  }

  @invalid_attrs %{email: nil, password: nil}

  describe "index" do
    test "lists all users", %{conn: conn} do
      user = user_fixture()

      conn =
        conn
        |> authenticate_user(user)
        |> get(~p"/api/v1/users")

      assert json_response(conn, 200)["success"] == true
      assert length(json_response(conn, 200)["data"]["users"]) == 1
    end

    test "supports pagination", %{conn: conn} do
      user = user_fixture()
      Enum.each(1..25, fn _ -> user_fixture() end)

      conn =
        conn
        |> authenticate_user(user)
        |> get(~p"/api/v1/users", %{page: 2, per_page: 10})

      response = json_response(conn, 200)
      assert response["success"] == true
      assert length(response["data"]["users"]) == 10
      assert response["data"]["pagination"]["page"] == 2
    end
  end

  describe "show" do
    test "shows chosen user", %{conn: conn} do
      user = user_fixture()

      conn =
        conn
        |> authenticate_user(user)
        |> get(~p"/api/v1/users/#{user.id}")

      assert json_response(conn, 200)["success"] == true
      assert json_response(conn, 200)["data"]["user"]["id"] == user.id
    end

    test "returns 404 for non-existent user", %{conn: conn} do
      user = user_fixture()

      conn =
        conn
        |> authenticate_user(user)
        |> get(~p"/api/v1/users/999")

      assert json_response(conn, 404)["success"] == false
    end
  end

  describe "create" do
    test "creates user when data is valid", %{conn: conn} do
      conn = post(conn, ~p"/api/v1/users", user: @create_attrs)

      assert %{"id" => id} = json_response(conn, 201)["data"]["user"]
      assert json_response(conn, 201)["success"] == true
    end

    test "renders errors when data is invalid", %{conn: conn} do
      conn = post(conn, ~p"/api/v1/users", user: @invalid_attrs)

      assert json_response(conn, 422)["success"] == false
      assert json_response(conn, 422)["error"]["details"] != %{}
    end
  end

  defp authenticate_user(conn, user) do
    token = MyApp.Token.generate_and_sign(user)
    put_req_header(conn, "authorization", "Bearer #{token}")
  end
end
```

#### Integration Tests
```elixir
# test/my_app_web/integration/api_workflow_test.exs
defmodule MyAppWeb.Integration.ApiWorkflowTest do
  use MyAppWeb.ConnCase, async: true

  import MyApp.AccountsFixtures

  test "complete user workflow", %{conn: conn} do
    # Create user
    conn = post(conn, ~p"/api/v1/users", user: %{
      email: "test@example.com",
      name: "Test User",
      password: "password123"
    })

    assert %{"id" => user_id} = json_response(conn, 201)["data"]["user"]

    # Login
    conn = post(conn, ~p"/api/v1/auth/login", %{
      email: "test@example.com",
      password: "password123"
    })

    assert %{"token" => token} = json_response(conn, 200)["data"]

    # Authenticated request
    conn =
      build_conn()
      |> put_req_header("authorization", "Bearer #{token}")
      |> get(~p"/api/v1/users/#{user_id}")

    assert json_response(conn, 200)["success"] == true

    # Update user
    conn =
      build_conn()
      |> put_req_header("authorization", "Bearer #{token}")
      |> put(~p"/api/v1/users/#{user_id}", user: %{name: "Updated Name"})

    assert json_response(conn, 200)["data"]["user"]["name"] == "Updated Name"
  end
end
```

### Phase 7: API Versioning

#### Version-Specific Controllers
```elixir
# lib/my_app_web/controllers/api/v2/user_controller.ex
defmodule MyAppWeb.Api.V2.UserController do
  use MyAppWeb.Api.BaseController

  # V2 has different response format
  def show(conn, %{"id" => id}) do
    with {:ok, user} <- Accounts.get_user(id) do
      # V2 includes additional fields and different structure
      render_success(conn, %{
        profile: %{
          id: user.id,
          personal_info: %{
            email: user.email,
            full_name: user.name,
            display_name: user.display_name
          },
          preferences: user.preferences,
          metadata: %{
            created: user.inserted_at,
            last_updated: user.updated_at,
            version: "v2"
          }
        }
      })
    end
  end
end
```

#### Content Negotiation
```elixir
# lib/my_app_web/plugs/api_version.ex
defmodule MyAppWeb.Plugs.ApiVersion do
  import Plug.Conn

  def init(default_version), do: default_version

  def call(conn, default_version) do
    version =
      get_version_from_header(conn) ||
      get_version_from_accept(conn) ||
      get_version_from_param(conn) ||
      default_version

    assign(conn, :api_version, version)
  end

  defp get_version_from_header(conn) do
    case get_req_header(conn, "api-version") do
      [version] -> version
      _ -> nil
    end
  end

  defp get_version_from_accept(conn) do
    case get_req_header(conn, "accept") do
      ["application/vnd.myapp.v" <> version <> "+json"] -> "v#{version}"
      _ -> nil
    end
  end

  defp get_version_from_param(conn) do
    conn.query_params["version"]
  end
end
```

### Phase 8: Performance Optimization

#### Response Caching
```elixir
# lib/my_app_web/plugs/cache.ex
defmodule MyAppWeb.Plugs.Cache do
  import Plug.Conn

  def init(opts), do: opts

  def call(conn, opts) do
    case get_cache_key(conn) do
      nil -> conn
      cache_key ->
        case Cachex.get(:api_cache, cache_key) do
          {:ok, nil} ->
            register_before_send(conn, fn conn ->
              if conn.status == 200 do
                ttl = Keyword.get(opts, :ttl, 300) # 5 minutes default
                Cachex.put(:api_cache, cache_key, {conn.resp_body, conn.resp_headers}, ttl: ttl)
              end
              conn
            end)
          {:ok, {body, headers}} ->
            conn
            |> merge_resp_headers(headers)
            |> put_resp_header("x-cache", "hit")
            |> send_resp(200, body)
            |> halt()
        end
    end
  end

  defp get_cache_key(conn) do
    if conn.method == "GET" do
      "#{conn.request_path}:#{conn.query_string}"
    end
  end
end
```

#### Database Query Optimization
```elixir
# lib/my_app/accounts.ex
defmodule MyApp.Accounts do
  import Ecto.Query, warn: false
  alias MyApp.Repo
  alias MyApp.Accounts.User

  def list_users(params \\ %{}) do
    User
    |> apply_filters(params)
    |> apply_pagination(params)
    |> preload([:profile, :posts])
    |> Repo.all()
  end

  defp apply_filters(query, params) do
    Enum.reduce(params, query, fn
      {"search", search}, query when search != "" ->
        from u in query,
        where: ilike(u.name, ^"%#{search}%") or ilike(u.email, ^"%#{search}%")

      {"role", role}, query when role != "" ->
        from u in query, where: u.role == ^role

      {"created_after", date}, query ->
        from u in query, where: u.inserted_at >= ^date

      _, query -> query
    end)
  end

  defp apply_pagination(query, params) do
    page = Map.get(params, "page", 1) |> String.to_integer()
    per_page = Map.get(params, "per_page", 20) |> String.to_integer()

    query
    |> limit(^per_page)
    |> offset(^((page - 1) * per_page))
  end
end
```

### Phase 9: Monitoring and Analytics

#### API Metrics Collection
```elixir
# lib/my_app_web/telemetry.ex
defmodule MyAppWeb.Telemetry do
  def init(_opts) do
    events = [
      [:phoenix, :endpoint, :stop],
      [:my_app, :api, :request]
    ]

    :telemetry.attach_many("my-app-telemetry", events, &handle_event/4, nil)
  end

  def handle_event([:phoenix, :endpoint, :stop], measurements, metadata, _config) do
    if String.starts_with?(metadata.request_path, "/api/") do
      :telemetry.execute(
        [:my_app, :api, :request],
        %{duration: measurements.duration},
        %{
          method: metadata.method,
          path: metadata.request_path,
          status: metadata.status,
          user_id: get_user_id(metadata.conn)
        }
      )
    end
  end

  defp get_user_id(conn) do
    case conn.assigns[:current_user] do
      nil -> "anonymous"
      user -> user.id
    end
  end
end
```

### Phase 10: Security Best Practices

#### Input Validation and Sanitization
```elixir
# lib/my_app_web/plugs/sanitize_params.ex
defmodule MyAppWeb.Plugs.SanitizeParams do
  import Plug.Conn

  def init(opts), do: opts

  def call(conn, _opts) do
    sanitized_params = sanitize_params(conn.params)
    %{conn | params: sanitized_params}
  end

  defp sanitize_params(params) when is_map(params) do
    Enum.into(params, %{}, fn {key, value} ->
      {key, sanitize_value(value)}
    end)
  end

  defp sanitize_value(value) when is_binary(value) do
    value
    |> String.trim()
    |> HtmlSanitizeEx.strip_tags()
  end

  defp sanitize_value(value) when is_map(value) do
    sanitize_params(value)
  end

  defp sanitize_value(value) when is_list(value) do
    Enum.map(value, &sanitize_value/1)
  end

  defp sanitize_value(value), do: value
end
```

#### CORS Configuration
```elixir
# lib/my_app_web/plugs/cors.ex
defmodule MyAppWeb.Plugs.CORS do
  import Plug.Conn

  def init(opts), do: opts

  def call(conn, _opts) do
    conn
    |> put_resp_header("access-control-allow-origin", get_allowed_origin(conn))
    |> put_resp_header("access-control-allow-methods", "GET, POST, PUT, DELETE, OPTIONS")
    |> put_resp_header("access-control-allow-headers", "content-type, authorization, x-requested-with")
    |> put_resp_header("access-control-allow-credentials", "true")
    |> handle_preflight()
  end

  defp get_allowed_origin(conn) do
    origin = get_req_header(conn, "origin") |> List.first()

    allowed_origins = [
      "https://myapp.com",
      "https://app.myapp.com",
      "http://localhost:3000"  # Development only
    ]

    if origin in allowed_origins, do: origin, else: "null"
  end

  defp handle_preflight(conn) do
    if conn.method == "OPTIONS" do
      conn
      |> send_resp(200, "")
      |> halt()
    else
      conn
    end
  end
end
```

## API Development Checklist

**Planning Phase**:
- [ ] API architecture defined
- [ ] Authentication strategy chosen
- [ ] Rate limiting requirements identified
- [ ] Versioning strategy planned
- [ ] Documentation format selected

**Development Phase**:
- [ ] Controllers and routes implemented
- [ ] Authentication and authorization working
- [ ] Input validation and sanitization
- [ ] Error handling comprehensive
- [ ] Response serialization consistent

**Documentation Phase**:
- [ ] OpenAPI/Swagger documentation
- [ ] GraphQL schema documented
- [ ] Authentication flow documented
- [ ] Example requests and responses
- [ ] SDKs or client libraries

**Testing Phase**:
- [ ] Unit tests for controllers
- [ ] Integration tests for workflows
- [ ] Authentication tests
- [ ] Rate limiting tests
- [ ] Performance tests

**Production Phase**:
- [ ] Monitoring and metrics
- [ ] Error tracking configured
- [ ] Performance optimization
- [ ] Security audit completed
- [ ] Load testing performed

Remember: Great APIs are consistent, well-documented, secure, and performant. Focus on developer experience and maintainability.
