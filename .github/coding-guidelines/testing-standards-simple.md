# Testing Standards - GitHub Copilot Guidelines

Essential testing patterns for Phoenix applications with GitHub Copilot.

## Context Testing

**Guideline**: Test all public context functions with success and failure scenarios.

**Path Patterns**: `test/**/*.exs`

**✅ Comprehensive Context Tests**:
```elixir
defmodule MyApp.AccountsTest do
  use MyApp.DataCase
  alias MyApp.Accounts

  describe "create_user/1" do
    test "creates user with valid attributes" do
      attrs = %{name: "John", email: "john@example.com"}
      assert {:ok, user} = Accounts.create_user(attrs)
      assert user.name == "John"
    end

    test "returns error with invalid email" do
      attrs = %{name: "John", email: "invalid"}
      assert {:error, changeset} = Accounts.create_user(attrs)
      assert "has invalid format" in errors_on(changeset).email
    end
  end
end
```

## LiveView Testing

**Guideline**: Test LiveView interactions and real-time updates.

**✅ LiveView Integration Tests**:
```elixir
defmodule MyAppWeb.PostLive.IndexTest do
  use MyAppWeb.ConnCase
  import Phoenix.LiveViewTest

  test "displays posts and handles real-time updates", %{conn: conn} do
    post = insert(:post)

    {:ok, view, _html} = live(conn, ~p"/posts")
    assert has_element?(view, "#post-#{post.id}")

    # Test real-time update
    new_post = insert(:post)
    send(view.pid, {:post_created, new_post})
    assert has_element?(view, "#post-#{new_post.id}")
  end
end
```

## Test Data

**Guideline**: Use factories for consistent test data.

**✅ Factory Usage**:
```elixir
# test/support/factory.ex
defmodule MyApp.Factory do
  use ExMachina.Ecto, repo: MyApp.Repo

  def user_factory do
    %MyApp.User{
      name: "User Name",
      email: sequence(:email, &"user#{&1}@example.com")
    }
  end
end

# In tests
test "user creation" do
  user = insert(:user)
  assert user.email =~ "@example.com"
end
```

## Database Testing

**Guideline**: Use transactions for test isolation.

**✅ Test Configuration**:
```elixir
# test/support/data_case.ex
defmodule MyApp.DataCase do
  use ExUnit.CaseTemplate

  using do
    quote do
      alias MyApp.Repo
      import Ecto.Query
      import MyApp.DataCase
      import MyApp.Factory
    end
  end

  setup tags do
    pid = Ecto.Adapters.SQL.Sandbox.start_owner!(MyApp.Repo, shared: not tags[:async])
    on_exit(fn -> Ecto.Adapters.SQL.Sandbox.stop_owner(pid) end)
    :ok
  end
end
```
