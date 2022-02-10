+++
title = "Let's drop the framework: Build a simple API in Rust with Hyper - Part 1"
date = 2022-01-10
+++

It's time to not use a framework. Yet we need not really do everything from scratch.
Hyper has come to our rescue.

<!-- more -->

## Setup Hyper

You might have already got the familiar daily routine. Let us add some dependencies to our
`Cargo.toml`:

```toml
[dependencies]
hyper = { version = "0.14.16", features = ["full"] }
tokio = { version = "1.15.0", features = ["full"] }
```

Of course, we will also need something to serialize our response data to JSON without wasting
so much effort. Let us
add `serde` and `serde_json`.

```toml
serde = { version = "1.0.133", features = ["derive"] }
serde_json = "1.0.74"
```

## Echo from Hyper

Here is the content of our `main.rs`:

```rust
use hyper::service::{make_service_fn, service_fn};
use hyper::{Body, Request, Response, Server};
use std::{convert::Infallible, net::SocketAddr};

async fn handle(_: Request<Body>) -> Result<Response<Body>, Infallible> {
    Ok(Response::new("Hello, World!!".into()))
}

#[tokio::main]
async fn main() {
    let addr = SocketAddr::from(([127, 0, 0, 1], 8181));

    let make_svc = make_service_fn(|_conn| async { Ok::<_, Infallible>(service_fn(handle)) });

    let server = Server::bind(&addr).serve(make_svc);

    if let Err(e) = server.await {
        eprintln!("server error: {}", e);
    }
}
```

Enter `cargo run` on your terminal. Our web application is now serving at 127.0.0.1 on port 8181. Open a
web browser (you already did) and navigate to <http://localhost:8181>. You should see "Hello, world!!"
text appears on your screen, like this:

![Hello, world!!](/img/hyperrs-img1.png)

It works! But it is really boring.

But, now, we will be making it seem less boring. Let us add some dynamic content.

## Implement a visitor counter

Our counter starts at 0. It will increase by 1 whenever someone visit our web page.

First, let add one more dependency, `lazy_static` to hold our counter variable globally:

```toml
[dependencies]
...
lazy_static= "1.4.0"
```

Okay, now we can easily get the counter working:

```rust
#[macro_use]
extern crate lazy_static;

lazy_static! {
    static ref COUNTER: Mutex<u32> = Mutex::new(0);
}
```

Modify our `handle` function to add `1` to `COUNTER` for every request:

```rust
async fn handle(_: Request<Body>) -> Result<Response<Body>, Infallible> {
    *COUNTER.lock().unwrap() += 1;
    Ok(Response::new(
        format!("Number of visits:{}", *COUNTER.lock().unwrap()).into(),
    ))
}
```

Run our server again. Notice something wrong on your web browser? Our counter adds 2 to itself every
time we hit refresh. Find a way to launch `DevTools` in you browser. Click on `network` tab. Refresh
again.

![Hyper.rs network tab](/img/hyperrs-img2.png)

Try again with `curl`:

```text
C:\Users\hai>curl localhost:8181
Number of visits:9
C:\Users\hai>curl localhost:8181
Number of visits:10
C:\Users\hai>curl localhost:8181
Number of visits:11
C:\Users\hai>
```

It works correctly this time.

In the next part of this tutorial, I will talk about routing and route matching so that the counter does not update 
when `favicon.ico` is requested.
