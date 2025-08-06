# Phoenix Patterns - GitHub Copilot Guidelines

Brief Phoenix development patterns for GitHub Copilot code review.

## Phoenix Contexts

**Guideline**: Use Phoenix contexts for business logic, not controllers.

**Path Patterns**: `lib/**/*.ex`

**❌ Incorrect**:
```elixir
defmodule MyAppWeb.UserController do
  def create(conn, %{"user" => user_params}) do
    # Business logic in controller
    changeset = %User{} |> User.changeset(user_params)
    case Repo.insert(changeset) do
      {:ok, user} -> send_welcome_email(user)
    end
  end
end
```

**✅ Correct**:
```elixir
defmodule MyAppWeb.UserController do
  def create(conn, %{"user" => user_params}) do
    case Accounts.create_user(user_params) do
      {:ok, user} -> # handle success
    end
  end
end

defmodule MyApp.Accounts do
  def create_user(attrs) do
    %User{}
    |> User.changeset(attrs)
    |> Repo.insert()
    |> case do
      {:ok, user} = result ->
        send_welcome_email(user)
        result
    end
  end
end
```

## LiveView Patterns

**Guideline**: Prefer LiveView for interactive UIs.

**Path Patterns**: `lib/*_web/**/*.ex`

**✅ LiveView for Real-time Updates**:
```elixir
defmodule MyAppWeb.PostLive.Index do
  use MyAppWeb, :live_view

  def mount(_params, _session, socket) do
    if connected?(socket), do: Blog.subscribe()
    posts = Blog.list_posts()
    {:ok, assign(socket, posts: posts)}
  end

  def handle_info({:post_created, post}, socket) do
    {:noreply, update(socket, :posts, &[post | &1])}
  end
end
```

## Error Handling

**Guideline**: Use `with` statements for complex error handling.

**✅ With Statements**:
```elixir
def process_payment(user_id, amount) do
  with {:ok, user} <- Accounts.get_user(user_id),
       {:ok, payment} <- Payments.charge(user, amount),
       {:ok, _} <- Notifications.send_receipt(user, payment) do
    {:ok, payment}
  else
    {:error, :user_not_found} -> {:error, "User not found"}
    {:error, :payment_failed} -> {:error, "Payment processing failed"}
    error -> error
  end
end
```

## Ecto Patterns

**Guideline**: Always preload associations, use changesets for validation.

**✅ Optimized Queries**:
```elixir
def list_posts_with_comments do
  from(p in Post,
    preload: [comments: [:user], :tags],
    order_by: [desc: p.inserted_at]
  )
  |> Repo.all()
end
```

**✅ Proper Changesets**:
```elixir
def changeset(user, attrs) do
  user
  |> cast(attrs, [:name, :email])
  |> validate_required([:name, :email])
  |> validate_format(:email, ~r/@/)
  |> unique_constraint(:email)
end
```
  def handle_event("save", %{"post" => post_params}, socket) do
    case Blog.update_post(socket.assigns.post, post_params) do
      {:ok, post} ->
        {:noreply, push_navigate(socket, to: ~p"/posts/#{post}")}
      {:error, changeset} ->
        {:noreply, assign(socket, form: to_form(changeset))}
    end
  end
end
```

## Pattern Matching and Error Handling

### Guideline: Always Use Pattern Matching in Function Heads
**Description**: Use pattern matching in function definitions and case statements for better error handling and code clarity.

**Path Patterns**: `lib/**/*.ex`, `test/**/*.exs`

**Examples**:

**❌ Incorrect - Generic parameter handling**:
```elixir
def process_user(user) do
  if user do
    if user.active do
      # process active user
    else
      {:error, "User not active"}
    end
  else
    {:error, "User not found"}
  end
end
```

**✅ Correct - Pattern matching**:
```elixir
def process_user(%User{active: true} = user) do
  # process active user
  {:ok, result}
end

def process_user(%User{active: false}) do
  {:error, "User not active"}
end

def process_user(nil) do
  {:error, "User not found"}
end
```

### Guideline: Use with Statements for Error Handling
**Description**: Use with statements for complex error handling chains instead of nested case statements.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - Nested case statements**:
```elixir
def create_post_with_tags(user_id, post_attrs, tag_names) do
  case Accounts.get_user(user_id) do
    {:ok, user} ->
      case Blog.create_post(user, post_attrs) do
        {:ok, post} ->
          case Tags.create_tags(tag_names) do
            {:ok, tags} ->
              Blog.associate_tags(post, tags)
            {:error, reason} -> {:error, reason}
          end
        {:error, reason} -> {:error, reason}
      end
    {:error, reason} -> {:error, reason}
  end
end
```

**✅ Correct - with statement**:
```elixir
def create_post_with_tags(user_id, post_attrs, tag_names) do
  with {:ok, user} <- Accounts.get_user(user_id),
       {:ok, post} <- Blog.create_post(user, post_attrs),
       {:ok, tags} <- Tags.create_tags(tag_names),
       {:ok, _associations} <- Blog.associate_tags(post, tags) do
    {:ok, post}
  end
end
```

## Documentation Patterns

### Guideline: Include @doc and @spec for Public Functions
**Description**: All public functions must include @doc documentation and @spec type specifications.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - Missing documentation**:
```elixir
def create_user(attrs) do
  %User{}
  |> User.changeset(attrs)
  |> Repo.insert()
end
```

**✅ Correct - Comprehensive documentation**:
```elixir
@doc """
Creates a new user with the given attributes.

## Parameters
- `attrs`: A map containing user attributes (email, name, etc.)

## Returns
- `{:ok, %User{}}` on success
- `{:error, %Ecto.Changeset{}}` on validation errors

## Examples
    iex> create_user(%{email: "user@example.com", name: "John"})
    {:ok, %User{id: 1, email: "user@example.com", name: "John"}}

    iex> create_user(%{email: "invalid"})
    {:error, %Ecto.Changeset{}}
"""
@spec create_user(map()) :: {:ok, User.t()} | {:error, Ecto.Changeset.t()}
def create_user(attrs) do
  %User{}
  |> User.changeset(attrs)
  |> Repo.insert()
end
```

## Database and Ecto Patterns

### Guideline: Always Use Ecto Changesets for Data Validation
**Description**: Use Ecto changesets for all data validation and transformation, never skip validation.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - Direct database insertion**:
```elixir
def create_user(attrs) do
  user = struct(User, attrs)
  Repo.insert(user)
end
```

**✅ Correct - Changeset validation**:
```elixir
def create_user(attrs) do
  %User{}
  |> User.changeset(attrs)
  |> Repo.insert()
end

# In the schema
def changeset(user, attrs) do
  user
  |> cast(attrs, [:email, :name])
  |> validate_required([:email, :name])
  |> validate_format(:email, ~r/@/)
  |> unique_constraint(:email)
end
```

### Guideline: Preload Associations Explicitly
**Description**: Always preload Ecto associations explicitly to avoid N+1 queries.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - N+1 query risk**:
```elixir
def list_posts do
  Repo.all(Post)
end

# Later in the code, this causes N+1 queries
posts = list_posts()
Enum.map(posts, fn post ->
  post.user.name  # This triggers a query for each post
end)
```

**✅ Correct - Explicit preloading**:
```elixir
def list_posts do
  Post
  |> preload([:user, :tags])
  |> Repo.all()
end

# Or with specific preloading
def list_posts_with_authors do
  Post
  |> join(:inner, [p], u in assoc(p, :user))
  |> preload([p, u], user: u)
  |> Repo.all()
end
```

## Testing Patterns

### Guideline: Test Business Logic Comprehensively
**Description**: All context functions must have corresponding tests covering success and failure scenarios.

**Path Patterns**: `test/**/*.exs`

**Examples**:

**❌ Incorrect - Incomplete testing**:
```elixir
test "create_user/1 creates a user" do
  attrs = %{email: "test@example.com", name: "Test"}
  assert {:ok, user} = Accounts.create_user(attrs)
end
```

**✅ Correct - Comprehensive testing**:
```elixir
describe "create_user/1" do
  test "creates user with valid attributes" do
    attrs = %{email: "test@example.com", name: "Test User"}

    assert {:ok, user} = Accounts.create_user(attrs)
    assert user.email == "test@example.com"
    assert user.name == "Test User"
  end

  test "returns error with invalid email" do
    attrs = %{email: "invalid", name: "Test User"}

    assert {:error, changeset} = Accounts.create_user(attrs)
    assert "has invalid format" in errors_on(changeset).email
  end

  test "returns error with missing required fields" do
    attrs = %{}

    assert {:error, changeset} = Accounts.create_user(attrs)
    assert "can't be blank" in errors_on(changeset).email
    assert "can't be blank" in errors_on(changeset).name
  end

  test "returns error with duplicate email" do
    email = "test@example.com"
    user_fixture(%{email: email})

    attrs = %{email: email, name: "Another User"}
    assert {:error, changeset} = Accounts.create_user(attrs)
    assert "has already been taken" in errors_on(changeset).email
  end
end
```

## Security Patterns

### Guideline: Sanitize User Input
**Description**: All user input must be sanitized and validated before processing.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - Direct HTML rendering**:
```elixir
def create_post(user, attrs) do
  %Post{user_id: user.id}
  |> Post.changeset(attrs)
  |> Repo.insert()
end

# In template: <%= raw @post.content %> # Dangerous!
```

**✅ Correct - Input sanitization**:
```elixir
def create_post(user, attrs) do
  %Post{user_id: user.id}
  |> Post.changeset(attrs)
  |> sanitize_content()
  |> Repo.insert()
end

defp sanitize_content(changeset) do
  case get_field(changeset, :content) do
    nil -> changeset
    content ->
      sanitized = HtmlSanitizeEx.strip_tags(content)
      put_change(changeset, :content, sanitized)
  end
end
```

These patterns ensure that GitHub Copilot provides consistent, secure, and maintainable code suggestions for Phoenix applications.
