# RawSocketUpgrade

A Rust library for handling WebSocket upgrades with raw socket access, based on Axum's WebSocket upgrade mechanism.


This websocket upgrade is based on the axum integrated one ([axum::extract::ws::WebSocketUpgrade])[https://docs.rs/axum/0.8.3/axum/extract/struct.WebSocketUpgrade.html]. The main difference is that it will onvoke the on_upgrade callback with the raw socket which allow the socket to be used by other libraries than the default tokio-tungstenite.

## Features

- Provides a `RawSocketUpgrade` struct for WebSocket upgrades.
- Allows direct access to the raw socket for use with libraries other than `tokio-tungstenite`.
- Supports both HTTP/1.1 (`GET` method) and HTTP/2+ (`CONNECT` method) WebSocket upgrades.
- Customizable error handling with the `OnFailedUpgrade` trait.

## Installation

Add the following to your `Cargo.toml`:

```toml
[dependencies]
axum-raw-websocket = { git = "https://github.com/tompro/axum-raw-websocket.git" }
```

## Usage

Here is an example of how to use `RawSocketUpgrade`:

```rust
use axum::{
    routing::get,
    Router,
};
use raw_socket_upgrade::RawSocketUpgrade;

async fn websocket_handler(upgrade: RawSocketUpgrade) -> axum::response::Response {
    upgrade.on_upgrade(|socket| async move {
        // Use the raw socket here
        println!("WebSocket connection established!");
    })
}

#[tokio::main]
async fn main() {
    let app = Router::new().route("/ws", get(websocket_handler));

    axum::Server::bind(&"0.0.0.0:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}
```

## Documentation

For detailed documentation, visit [docs.rs](https://docs.rs).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
