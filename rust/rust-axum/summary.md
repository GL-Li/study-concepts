## Axum framework
Key idea:????????????????
- Define a with_state
- Define handlers to process the state



### Axum server entry point

```rust
let state = AppState {...};
let app = Router::new()
    .route("/", get(root)).
    rounte("/notes", get(list_notes).post(create_note))..
    .with_state(state);
let listener = tokio::net::TcpListener::bind("127.0.0.1:3000").await.unwrap();
axum::serve(listener, app).await.unwrap();
```

### Router
A type for composing handlers and services.

```rust
let app = Router::new()
    .route("/", get(root))
    // add one GET and one POST endpoint at the same path
    // get(list_notes) returns a MethodRouter with its GET slot
    // filled. The .post(create_note) fill the POST slot of the
    // the same MethodRouter
    .route("/notes", get(list_notes).post(create_note))
    .route("/notes/{id}", get(get_note))
    .with_state(state);
```

### Handlers
A handler is an `async fn` whose arguments are extractors and whose return type implements `IntoResponse`.

### Extractor
Extractors are types for how handler read the request -- path, query, body, headers, or shared state -- all in the function signature.

Built-in extractors: `State`, `Path`, `Query`, `Json`, `Request`, ...

### Path parameters

