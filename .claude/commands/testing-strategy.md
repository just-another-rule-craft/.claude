# Phoenix Testing Strategy

Comprehensive testing approach for Phoenix applications covering all layers and scenarios.

**Usage**: `/project:testing-strategy [testing focus area]`

**Arguments**:
- `$ARGUMENTS`: Specific testing area (unit, integration, e2e, performance, etc.)

## Workflow

Implement comprehensive testing for: $ARGUMENTS

Follow this systematic testing approach:

### Phase 1: Testing Strategy Planning
Use "think hard" to design the testing approach:
- What are the critical user journeys?
- What are the high-risk areas?
- What is the current test coverage?
- What types of testing are needed?
- What is the testing pyramid structure?

### Phase 2: Test Environment Setup

#### Test Configuration
```
Use `get_source_location` to examine:
- Test environment configuration
- Database setup for testing
- Mock configurations
- Test data factories
- Testing dependencies
```

#### Essential Testing Libraries
```elixir
# In mix.exs deps
{:ex_machina, "~> 2.7", only: :test}      # Factories
{:mox, "~> 1.0", only: :test}             # Mocking
{:wallaby, "~> 0.30", only: :test}        # E2E testing
{:bypass, "~> 2.1", only: :test}          # HTTP client testing
{:stream_data, "~> 0.5", only: :test}     # Property-based testing
```

### Phase 3: Unit Testing Strategy

#### Context Testing (Business Logic)
```elixir
# Test all context functions thoroughly
defmodule MyApp.AccountsTest do
  use MyApp.DataCase
  alias MyApp.Accounts

  describe "create_user/1" do
    test "creates user with valid attributes" do
      attrs = %{email: "test@example.com", password: "password123"}
      assert {:ok, user} = Accounts.create_user(attrs)
      assert user.email == "test@example.com"
    end

    test "returns error with invalid attributes" do
      attrs = %{email: "invalid", password: "short"}
      assert {:error, changeset} = Accounts.create_user(attrs)
      assert %{email: ["has invalid format"]} = errors_on(changeset)
    end

    test "hashes password on creation" do
      attrs = %{email: "test@example.com", password: "password123"}
      assert {:ok, user} = Accounts.create_user(attrs)
      assert Bcrypt.verify_pass("password123", user.password_hash)
    end
  end
end
```

#### Schema Testing
```elixir
# Test all validations and constraints
defmodule MyApp.UserTest do
  use MyApp.DataCase
  alias MyApp.Accounts.User

  describe "changeset/2" do
    test "validates email format" do
      changeset = User.changeset(%User{}, %{email: "invalid"})
      assert %{email: ["has invalid format"]} = errors_on(changeset)
    end

    test "validates password length" do
      changeset = User.changeset(%User{}, %{password: "short"})
      assert %{password: ["should be at least 8 character(s)"]} = errors_on(changeset)
    end
  end
end
```

### Phase 4: Integration Testing

#### Phoenix Controller Testing
```elixir
defmodule MyAppWeb.UserControllerTest do
  use MyAppWeb.ConnCase
  import MyApp.AccountsFixtures

  describe "GET /users" do
    test "lists all users", %{conn: conn} do
      user = user_fixture()
      conn = get(conn, ~p"/users")
      assert html_response(conn, 200) =~ user.email
    end
  end

  describe "POST /users" do
    test "creates user with valid data", %{conn: conn} do
      attrs = %{email: "test@example.com", password: "password123"}
      conn = post(conn, ~p"/users", user: attrs)
      assert redirected_to(conn) == ~p"/users"
    end
  end
end
```

#### LiveView Testing
```elixir
defmodule MyAppWeb.UserLiveTest do
  use MyAppWeb.ConnCase
  import Phoenix.LiveViewTest
  import MyApp.AccountsFixtures

  describe "Index" do
    test "lists all users", %{conn: conn} do
      user = user_fixture()
      {:ok, _index_live, html} = live(conn, ~p"/users")
      assert html =~ "Listing Users"
      assert html =~ user.email
    end

    test "deletes user", %{conn: conn} do
      user = user_fixture()
      {:ok, index_live, _html} = live(conn, ~p"/users")

      assert index_live |> element("#users-#{user.id} a", "Delete") |> render_click()
      refute has_element?(index_live, "#user-#{user.id}")
    end
  end
end
```

### Phase 5: End-to-End Testing with Wallaby

#### User Journey Testing
```elixir
defmodule MyAppWeb.UserJourneyTest do
  use MyAppWeb.FeatureCase, async: true
  import Wallaby.Query

  feature "user registration and login", %{session: session} do
    session
    |> visit("/")
    |> click(link("Sign Up"))
    |> fill_in(text_field("Email"), with: "test@example.com")
    |> fill_in(text_field("Password"), with: "password123")
    |> click(button("Create Account"))
    |> assert_has(text("Welcome"))
  end

  feature "authenticated user workflow", %{session: session} do
    user = user_fixture()

    session
    |> log_in_user(user)
    |> visit("/dashboard")
    |> assert_has(text("Dashboard"))
    |> click(link("Create Post"))
    |> fill_in(text_field("Title"), with: "Test Post")
    |> click(button("Publish"))
    |> assert_has(text("Post created successfully"))
  end
end
```

### Phase 6: API Testing

#### JSON API Testing
```elixir
defmodule MyAppWeb.ApiUserControllerTest do
  use MyAppWeb.ConnCase
  import MyApp.AccountsFixtures

  describe "GET /api/users" do
    test "returns users list", %{conn: conn} do
      user = user_fixture()
      conn =
        conn
        |> put_req_header("accept", "application/json")
        |> get(~p"/api/users")

      assert json_response(conn, 200)["data"] |> length() == 1
    end
  end

  describe "POST /api/users" do
    test "creates user with valid data", %{conn: conn} do
      attrs = %{email: "test@example.com", password: "password123"}

      conn =
        conn
        |> put_req_header("accept", "application/json")
        |> post(~p"/api/users", user: attrs)

      assert %{"id" => id} = json_response(conn, 201)["data"]
    end
  end
end
```

#### GraphQL Testing (if applicable)
```elixir
defmodule MyAppWeb.Schema.UserTest do
  use MyAppWeb.ConnCase
  import MyApp.AccountsFixtures

  describe "users query" do
    test "returns all users" do
      user = user_fixture()

      query = """
      query {
        users {
          id
          email
        }
      }
      """

      conn = build_conn() |> post("/api/graphql", %{query: query})
      assert %{"data" => %{"users" => [user_data]}} = json_response(conn, 200)
      assert user_data["email"] == user.email
    end
  end
end
```

### Phase 7: Performance Testing

#### Load Testing Setup
```elixir
defmodule MyApp.LoadTest do
  use ExUnit.Case

  @moduletag :load_test

  test "handles concurrent user creation" do
    tasks = 1..100
    |> Enum.map(fn i ->
      Task.async(fn ->
        attrs = %{email: "user#{i}@example.com", password: "password123"}
        MyApp.Accounts.create_user(attrs)
      end)
    end)

    results = Enum.map(tasks, &Task.await(&1, 10_000))
    successful_creates = Enum.count(results, &match?({:ok, _}, &1))

    assert successful_creates >= 95  # 95% success rate
  end
end
```

#### Database Performance Testing
```elixir
defmodule MyApp.DatabasePerformanceTest do
  use MyApp.DataCase

  test "user queries perform within acceptable limits" do
    # Create test data
    1..1000 |> Enum.each(fn i ->
      user_fixture(%{email: "user#{i}@example.com"})
    end)

    # Test query performance
    {time, _result} = :timer.tc(fn ->
      MyApp.Accounts.list_users()
    end)

    # Should complete within 100ms
    assert time < 100_000  # microseconds
  end
end
```

### Phase 8: Property-Based Testing

#### Data Validation Testing
```elixir
defmodule MyApp.UserPropertyTest do
  use ExUnit.Case
  use ExUnitProperties
  alias MyApp.Accounts.User

  property "user changeset handles any string email" do
    check all email <- string(:printable) do
      changeset = User.changeset(%User{}, %{email: email})

      if String.contains?(email, "@") and String.contains?(email, ".") do
        assert changeset.valid?
      else
        refute changeset.valid?
      end
    end
  end
end
```

### Phase 9: Security Testing

#### Authentication Testing
```elixir
defmodule MyAppWeb.AuthTest do
  use MyAppWeb.ConnCase
  import MyApp.AccountsFixtures

  describe "authentication" do
    test "blocks unauthenticated access to protected routes", %{conn: conn} do
      conn = get(conn, ~p"/dashboard")
      assert redirected_to(conn) == ~p"/login"
    end

    test "prevents session fixation attacks", %{conn: conn} do
      # Test session regeneration on login
      user = user_fixture()

      conn = post(conn, ~p"/login", %{email: user.email, password: "password123"})
      old_session_id = get_session(conn, :session_id)

      conn = post(conn, ~p"/login", %{email: user.email, password: "password123"})
      new_session_id = get_session(conn, :session_id)

      refute old_session_id == new_session_id
    end
  end
end
```

#### Authorization Testing
```elixir
defmodule MyAppWeb.AuthorizationTest do
  use MyAppWeb.ConnCase
  import MyApp.AccountsFixtures

  describe "role-based access" do
    test "admin can access admin routes", %{conn: conn} do
      admin = user_fixture(%{role: :admin})
      conn = log_in_user(conn, admin)

      conn = get(conn, ~p"/admin")
      assert html_response(conn, 200)
    end

    test "regular user cannot access admin routes", %{conn: conn} do
      user = user_fixture(%{role: :user})
      conn = log_in_user(conn, user)

      conn = get(conn, ~p"/admin")
      assert redirected_to(conn) == ~p"/"
    end
  end
end
```

### Phase 10: Test Data Management

#### Factory Setup with ExMachina
```elixir
defmodule MyApp.AccountsFixtures do
  def user_fixture(attrs \\ %{}) do
    {:ok, user} =
      attrs
      |> Enum.into(%{
        email: "user#{System.unique_integer()}@example.com",
        password: "password123",
        role: :user
      })
      |> MyApp.Accounts.create_user()

    user
  end
end
```

#### Database State Management
```elixir
defmodule MyApp.DataCase do
  use ExUnit.CaseTemplate

  using do
    quote do
      alias MyApp.Repo
      import Ecto
      import Ecto.Changeset
      import Ecto.Query
      import MyApp.DataCase
    end
  end

  setup tags do
    MyApp.DataCase.setup_sandbox(tags)
    :ok
  end

  def setup_sandbox(tags) do
    pid = Ecto.Adapters.SQL.Sandbox.start_owner!(MyApp.Repo, shared: not tags[:async])
    on_exit(fn -> Ecto.Adapters.SQL.Sandbox.stop_owner(pid) end)
  end
end
```

## Testing Best Practices

### Test Organization
- Group related tests with `describe` blocks
- Use descriptive test names that explain expected behavior
- Follow the Arrange-Act-Assert pattern
- Keep tests focused on single behaviors

### Test Data
- Use factories for consistent test data
- Avoid shared state between tests
- Use database transactions for isolation
- Clean up external resources

### Mocking and Stubbing
```elixir
# Use Mox for behavior-based mocking
defmodule MyApp.EmailMock do
  use Mox
  def send_email(_to, _subject, _body), do: {:ok, "sent"}
end

# Stub external services
test "sends welcome email on user creation" do
  expect(MyApp.EmailMock, :send_email, fn _, "Welcome", _ -> {:ok, "sent"} end)

  {:ok, _user} = MyApp.Accounts.create_user(%{email: "test@example.com"})

  verify!(MyApp.EmailMock)
end
```

### Continuous Testing
```bash
# Run tests on file changes
mix test.watch

# Run tests with coverage
mix test --cover

# Run specific test files
mix test test/myapp/accounts_test.exs

# Run tests matching pattern
mix test --only user_creation
```

## Testing Checklist

**Unit Tests**:
- [ ] All context functions tested
- [ ] Schema validations covered
- [ ] Edge cases and error conditions tested
- [ ] Pure functions isolated from side effects

**Integration Tests**:
- [ ] All controller actions tested
- [ ] LiveView interactions tested
- [ ] Authentication and authorization tested
- [ ] API endpoints validated

**End-to-End Tests**:
- [ ] Critical user journeys tested
- [ ] Cross-browser compatibility verified
- [ ] Mobile responsiveness tested
- [ ] Error scenarios handled

**Performance Tests**:
- [ ] Load testing for critical paths
- [ ] Database query performance verified
- [ ] Memory usage monitored
- [ ] Concurrent access tested

**Security Tests**:
- [ ] Authentication bypass attempts
- [ ] Authorization escalation tests
- [ ] Input validation security
- [ ] Session security verified

Remember: Good tests are investments in your application's future. They should be fast, reliable, and provide confidence in your code's correctness.
