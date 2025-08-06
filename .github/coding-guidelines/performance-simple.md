# Performance Optimization - GitHub Copilot Guidelines

Essential performance patterns for Phoenix applications.

## Database Optimization

**Guideline**: Prevent N+1 queries with proper preloading.

**❌ N+1 Problem**:
```elixir
# This causes N+1 queries
posts = Blog.list_posts()  # 1 query

# Each iteration causes another query
for post <- posts do
  IO.puts(post.user.name)  # N queries
end
```

**✅ Optimized with Preloading**:
```elixir
def list_posts_with_users do
  from(p in Post,
    preload: [:user, :comments],
    order_by: [desc: p.inserted_at]
  )
  |> Repo.all()
end
```

## Memory Management

**Guideline**: Use streaming for large datasets.

**❌ Memory Intensive**:
```elixir
def export_all_users do
  users = Repo.all(User)  # Loads all into memory
  Enum.map(users, &format_user/1)
end
```

**✅ Memory Efficient**:
```elixir
def export_users_stream do
  User
  |> Repo.stream(max_rows: 1000)
  |> Stream.map(&format_user/1)
  |> Stream.run()
end
```

## LiveView Performance

**Guideline**: Minimize re-renders and optimize state.

**✅ Optimized LiveView**:
```elixir
defmodule MyAppWeb.PostLive.Index do
  use MyAppWeb, :live_view

  def mount(_params, _session, socket) do
    if connected?(socket) do
      Blog.subscribe_to_posts()
    end

    socket =
      socket
      |> assign(:loading, false)
      |> load_posts()

    {:ok, socket}
  end

  def handle_info({:post_created, post}, socket) do
    # Only update if post matches current filters
    if post_matches_filter?(post, socket.assigns.filter) do
      posts = [post | socket.assigns.posts]
      {:noreply, assign(socket, posts: posts)}
    else
      {:noreply, socket}
    end
  end
end
```

## Caching

**Guideline**: Cache expensive operations appropriately.

**✅ Simple Caching**:
```elixir
def get_popular_posts do
  case Cache.get("popular_posts") do
    nil ->
      posts = expensive_query_for_popular_posts()
      Cache.put("popular_posts", posts, ttl: :timer.minutes(30))
      posts

    cached_posts ->
      cached_posts
  end
end
```

## Background Jobs

**Guideline**: Use background jobs for heavy operations.

**✅ Non-blocking Operations**:
```elixir
defmodule MyAppWeb.ReportController do
  def generate_report(conn, params) do
    user = conn.assigns.current_user

    # Start background job
    MyApp.ReportWorker.new(%{
      user_id: user.id,
      report_type: params["type"]
    })
    |> Oban.insert()

    json(conn, %{
      message: "Report generation started",
      status: "processing"
    })
  end
end
```
