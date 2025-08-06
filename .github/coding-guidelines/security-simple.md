# Security Practices - GitHub Copilot Guidelines

Essential security patterns for Phoenix applications.

## Input Validation

**Guideline**: Sanitize and validate all user input.

**Path Patterns**: `lib/**/*.ex`

**✅ Secure Input Handling**:
```elixir
def create_post(user, attrs) do
  %Post{user_id: user.id}
  |> Post.changeset(attrs)
  |> validate_content_safety()
  |> Repo.insert()
end

defp validate_content_safety(changeset) do
  content = get_field(changeset, :content)

  if content && contains_suspicious_patterns?(content) do
    add_error(changeset, :content, "contains potentially harmful content")
  else
    changeset
  end
end
```

## Authentication & Authorization

**Guideline**: Always verify user permissions before allowing access.

**✅ Proper Authorization**:
```elixir
defmodule MyAppWeb.UserController do
  plug :authenticate_user
  plug :authorize_user_access when action in [:show, :update]

  defp authorize_user_access(conn, _opts) do
    user_id = conn.params["id"]
    current_user = conn.assigns.current_user

    if can_access_user?(current_user, user_id) do
      conn
    else
      conn
      |> put_status(:forbidden)
      |> json(%{error: "Access denied"})
      |> halt()
    end
  end
end
```

## CSRF Protection

**Guideline**: Use CSRF protection on all forms.

**✅ Secure Forms**:
```elixir
# In router.ex
pipeline :browser do
  plug :protect_from_forgery
  plug :put_secure_browser_headers
end

# In templates
<.form for={@form} phx-submit="save">
  <!-- CSRF token automatically included -->
  <.input field={@form[:name]} label="Name" />
  <.button>Save</.button>
</.form>
```

## Database Security

**Guideline**: Use parameterized queries, never string interpolation.

**❌ Dangerous**:
```elixir
def search_users(query) do
  sql = "SELECT * FROM users WHERE name LIKE '%#{query}%'"
  Repo.query!(sql)  # SQL injection vulnerability!
end
```

**✅ Secure**:
```elixir
def search_users(query) do
  from(u in User,
    where: ilike(u.name, ^"%#{query}%"),
    limit: 50
  )
  |> Repo.all()
end
```

## Session Security

**Guideline**: Configure secure session settings.

**✅ Secure Configuration**:
```elixir
# config/config.exs
config :my_app, MyAppWeb.Endpoint,
  session: [
    store: :cookie,
    key: "_my_app_session",
    signing_salt: System.get_env("SESSION_SIGNING_SALT"),
    max_age: 60 * 60 * 24 * 7,  # 1 week
    secure: true,               # HTTPS only
    http_only: true,            # Prevent XSS
    same_site: "Lax"           # CSRF protection
  ]
```
