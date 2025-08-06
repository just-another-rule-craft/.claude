# Elixir Style - GitHub Copilot Guidelines

Essential Elixir style guidelines for GitHub Copilot code review.

## Function Definitions

**Guideline**: Use pattern matching in function heads, include docs for public functions.

**Path Patterns**: `lib/**/*.ex`

**✅ Pattern Matching**:
```elixir
@doc "Processes user based on status"
@spec process_user(User.t()) :: {:ok, User.t()} | {:error, String.t()}
def process_user(%User{status: :active} = user) do
  # Handle active user
  {:ok, user}
end

def process_user(%User{status: :inactive}) do
  {:error, "User is inactive"}
end
```

## Data Structures

**Guideline**: Prefer structs over maps for domain data.

**✅ Using Structs**:
```elixir
defmodule User do
  @enforce_keys [:email]
  defstruct [:id, :email, :name, :inserted_at]

  @type t :: %__MODULE__{
    id: String.t() | nil,
    email: String.t(),
    name: String.t() | nil,
    inserted_at: DateTime.t() | nil
  }
end
```

## Error Handling

**Guideline**: Use tagged tuples and pattern matching for error handling.

**✅ Tagged Tuples**:
```elixir
def fetch_user(id) do
  case Repo.get(User, id) do
    nil -> {:error, :not_found}
    user -> {:ok, user}
  end
end

# Usage
case fetch_user(123) do
  {:ok, user} -> process_user(user)
  {:error, :not_found} -> handle_not_found()
end
```

## Pipe Operator

**Guideline**: Use pipe operator for data transformations, avoid in conditionals.

**✅ Good Piping**:
```elixir
def process_users(users) do
  users
  |> Enum.filter(&(&1.active))
  |> Enum.map(&transform_user/1)
  |> Enum.sort_by(&(&1.name))
end
```

**❌ Avoid in Conditionals**:
```elixir
# Don't do this
result = user |> User.valid?() |> if do
  process_user(user)
else
  {:error, "Invalid user"}
end
```

  defp hash_password(password), do: # ...

  def get_user(id), do: # ...
end
```

**✅ Correct organization**:
```elixir
defmodule MyApp.Accounts do
  # Public functions (alphabetically)
  def create_user(attrs), do: # ...

  def get_user(id), do: # ...

  def update_user(user, attrs), do: # ...

  # Private functions (alphabetically)
  defp hash_password(password), do: # ...

  defp validate_email(email), do: # ...
end
```

## Pattern Matching Style

### Guideline: Use Descriptive Pattern Matching
**Description**: Make pattern matching explicit and descriptive. Avoid catch-all patterns when specific patterns are more appropriate.

**Path Patterns**: `lib/**/*.ex`, `test/**/*.exs`

**Examples**:

**❌ Incorrect - Generic pattern matching**:
```elixir
def handle_user_result(result) do
  case result do
    {:ok, data} -> process_success(data)
    {:error, reason} -> handle_error(reason)
    _ -> {:error, :unknown}
  end
end
```

**✅ Correct - Descriptive pattern matching**:
```elixir
def handle_user_result({:ok, %User{} = user}) do
  process_successful_user_creation(user)
end

def handle_user_result({:error, %Ecto.Changeset{} = changeset}) do
  handle_validation_errors(changeset)
end

def handle_user_result({:error, :not_found}) do
  handle_user_not_found()
end

def handle_user_result({:error, reason}) when is_binary(reason) do
  handle_generic_error(reason)
end
```

## Guard Usage

### Guideline: Use Guards for Type and Value Validation
**Description**: Use guards to make function clauses more explicit and to fail fast on invalid input.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - Runtime validation**:
```elixir
def calculate_age(birth_date) do
  if is_binary(birth_date) or is_nil(birth_date) do
    {:error, "Invalid birth date"}
  else
    # calculation logic
  end
end
```

**✅ Correct - Guard clauses**:
```elixir
def calculate_age(%Date{} = birth_date) when birth_date <= Date.utc_today() do
  Date.diff(Date.utc_today(), birth_date) / 365
  |> trunc()
end

def calculate_age(%Date{} = birth_date) when birth_date > Date.utc_today() do
  {:error, "Birth date cannot be in the future"}
end

def calculate_age(_invalid_date) do
  {:error, "Invalid birth date format"}
end
```

## Documentation Style

### Guideline: Use Consistent Documentation Format
**Description**: Follow ExDoc conventions for module and function documentation.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect documentation**:
```elixir
defmodule MyApp.Accounts do
  # This module handles users

  # Gets a user
  def get_user(id) do
    # ...
  end
end
```

**✅ Correct documentation**:
```elixir
defmodule MyApp.Accounts do
  @moduledoc """
  The Accounts context.

  Handles user account management including creation, authentication,
  and profile management.
  """

  @doc """
  Gets a single user by ID.

  Returns `{:ok, user}` if the user exists, `{:error, :not_found}` otherwise.

  ## Examples

      iex> get_user(123)
      {:ok, %User{id: 123, email: "user@example.com"}}

      iex> get_user(999)
      {:error, :not_found}
  """
  @spec get_user(integer()) :: {:ok, User.t()} | {:error, :not_found}
  def get_user(id) when is_integer(id) do
    # ...
  end
end
```

## Error Handling Style

### Guideline: Use Consistent Error Tuples
**Description**: Return consistent error tuples with descriptive atoms and messages.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect error handling**:
```elixir
def create_user(attrs) do
  case User.changeset(%User{}, attrs) do
    %{valid?: true} = changeset ->
      Repo.insert(changeset)
    changeset ->
      "Validation failed"
  end
end
```

**✅ Correct error handling**:
```elixir
def create_user(attrs) do
  %User{}
  |> User.changeset(attrs)
  |> Repo.insert()
  |> case do
    {:ok, user} -> {:ok, user}
    {:error, changeset} -> {:error, changeset}
  end
end

# Or using with for multiple operations
def create_user_with_profile(user_attrs, profile_attrs) do
  with {:ok, user} <- create_user(user_attrs),
       {:ok, profile} <- create_profile(user, profile_attrs) do
    {:ok, %{user: user, profile: profile}}
  else
    {:error, %Ecto.Changeset{} = changeset} ->
      {:error, {:validation_failed, changeset}}
    {:error, reason} ->
      {:error, {:creation_failed, reason}}
  end
end
```

## Pipe Operator Usage

### Guideline: Use Pipes for Data Transformation Chains
**Description**: Use the pipe operator for clear data transformation chains, but avoid overly long pipes.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - Nested function calls**:
```elixir
def process_user_data(raw_data) do
  validate_data(normalize_data(parse_data(raw_data)))
end
```

**❌ Incorrect - Overly long pipe**:
```elixir
def process_user_data(raw_data) do
  raw_data
  |> parse_data()
  |> normalize_data()
  |> validate_data()
  |> transform_to_changeset()
  |> apply_business_rules()
  |> validate_against_external_service()
  |> finalize_processing()
  |> send_notifications()
  |> log_activity()
end
```

**✅ Correct - Balanced pipe usage**:
```elixir
def process_user_data(raw_data) do
  raw_data
  |> parse_data()
  |> normalize_data()
  |> validate_data()
  |> create_user_if_valid()
end

defp create_user_if_valid({:ok, processed_data}) do
  processed_data
  |> User.changeset()
  |> Repo.insert()
  |> handle_user_creation()
end

defp create_user_if_valid({:error, reason}), do: {:error, reason}
```

## Module Attribute Usage

### Guideline: Use Module Attributes for Constants and Configuration
**Description**: Use module attributes for compile-time constants and avoid magic numbers/strings.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - Magic numbers and strings**:
```elixir
defmodule MyApp.UserValidator do
  def validate_age(age) do
    if age >= 18 and age <= 120 do
      :ok
    else
      {:error, "Age must be between 18 and 120"}
    end
  end

  def validate_password(password) do
    if String.length(password) >= 8 do
      :ok
    else
      {:error, "Password too short"}
    end
  end
end
```

**✅ Correct - Module attributes**:
```elixir
defmodule MyApp.UserValidator do
  @min_age 18
  @max_age 120
  @min_password_length 8

  @age_error_message "Age must be between #{@min_age} and #{@max_age}"
  @password_error_message "Password must be at least #{@min_password_length} characters"

  def validate_age(age) when age >= @min_age and age <= @max_age, do: :ok
  def validate_age(_age), do: {:error, @age_error_message}

  def validate_password(password) when byte_size(password) >= @min_password_length, do: :ok
  def validate_password(_password), do: {:error, @password_error_message}
end
```

## Struct Definition Style

### Guideline: Define Structs with Clear Field Types and Defaults
**Description**: Use clear field definitions with appropriate defaults and type information.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect struct definition**:
```elixir
defmodule MyApp.User do
  defstruct [:id, :email, :name, :active, :settings]
end
```

**✅ Correct struct definition**:
```elixir
defmodule MyApp.User do
  @type t :: %__MODULE__{
    id: integer() | nil,
    email: String.t(),
    name: String.t() | nil,
    active: boolean(),
    settings: map(),
    inserted_at: DateTime.t() | nil,
    updated_at: DateTime.t() | nil
  }

  defstruct [
    :id,
    :email,
    :name,
    :inserted_at,
    :updated_at,
    active: true,
    settings: %{}
  ]
end
```

## Comment Style

### Guideline: Use Comments Sparingly and Meaningfully
**Description**: Write self-documenting code. Use comments only for complex business logic or non-obvious decisions.

**Path Patterns**: `lib/**/*.ex`

**Examples**:

**❌ Incorrect - Obvious comments**:
```elixir
def create_user(attrs) do
  # Create a changeset
  changeset = User.changeset(%User{}, attrs)

  # Insert into database
  case Repo.insert(changeset) do
    # If successful, return user
    {:ok, user} -> {:ok, user}
    # If error, return error
    {:error, changeset} -> {:error, changeset}
  end
end
```

**✅ Correct - Meaningful comments**:
```elixir
def calculate_user_score(user, activities) do
  base_score = user.reputation_points

  # Apply time decay: recent activities worth more
  # Score decreases by 10% for each month of inactivity
  time_factor = calculate_time_decay(user.last_active_at)

  # Bonus points for diverse activity types
  # Encourages users to engage with different features
  diversity_bonus = calculate_diversity_bonus(activities)

  (base_score * time_factor + diversity_bonus)
  |> max(0)  # Ensure score never goes negative
  |> round()
end
```

These style guidelines ensure that GitHub Copilot generates clean, consistent, and maintainable Elixir code that follows community best practices.
