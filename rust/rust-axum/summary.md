## Axum framework
All axum does is: receive a HTTP request and send back a HTTP response.

With a little more details: axum's core loop is, step by step:
1. receive HTTP request
2. match the request to a route/handler
3. run extractors to pull typed data out of the request
4. call the handler
5. convert handler's return value into an HTTP response



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
`Path<T>` turns the `{...}` declaired in the route into a typed Rust value, for example `{id}` in `"/notes/{id}`.

How `Path<T>` works, for `GET /notes/42`:
- Router matches the path against `.route("/notes/{id}", get(get_note))` (src/main.rs:115) and captures `id = "42"`.
- That capture map `{"id": "42"}` is attached to the request before the handler runs.
- `Path<u64>` uses the type u64 to drive deserialization of the single captured value: `"42".parse::<u64>() → 42`, yielding `Path(42).` The key "id" is only used for name-based targets (struct/HashMap); for a scalar it's ignored.
- `Path(id): Path<u64>` (src/main.rs:74) pattern-matches the tuple struct `Path(42),` binding `id = 42`. The name `id` is an arbitrary local binding, unrelated to `{id}`.

### Query parameters
`Query<T>` deserializes the URL's query string (everything after `?`) into struct `T`.

How `Query<T>` works for `GET /notes?limit=10`
- Router first match path `GET /notes` against `.router("/notes", get(list_notes)....)`
- `Query<ListQuery>` in handler `list_notes` captures the query string to `Query(ListQuery {limit: 10})`
- `Query(q)` pattern match to get `q = ListQuery {limit: 10}`
- note name `limit` in query string must match that in `ListQuery`' field.

### Json<T>: both a body extractor and a respone type

#### as an extractor
`Json<T>` extract the body of a request into type T.

How it works:
- Example request:
    ```
    POST /notes HTTP/1.1
    Host: 127.0.0.1.3000
    Content-type: application/Json  # must be this type

    {"title": "Groceries", "body": "mile, eggs"}
    ```
- This request maps to handler `create_note` in `.route("/notes", get(list_notes).post(create_note))`
- In the handler the extractor `Json<NewNote>` deserializes the body into `NewNote {title: "groceries".to_string(), body: "milk, eggs".to_string(),}`. Body keys need to match struct field.
- `Json(payload)` destructures `payload` to `NewNote {...}`

#### as a response type
`Json<T>` serializes type `T` as part of the response to a request.

How it works for request: `GET /notes/1`
- The request maps to `.route("notes/{id}", get(get_notes))`
- The handler `get_notes` locate the note and return `Ok(Json(note))` where `note` is `Note {id: 1, title: "Welcome".into(), body: "Your fist note".into()}`
- What axum sends back is a full response:
    ```
    HTTP/1.1 200 OK
    Content-type: application/Json

    {"id":1, "title":"Welcom", "body":"Your first note"}
    ```

### Shared state
This state is shared by each request with `Arc::clone` and is extracted by extractor `State<T>`. `State<T>` extractor is used in hadlers that depend on data in state.

The state extraction is straightforward with `State(state): State<T>` destructure in handler parameters.
