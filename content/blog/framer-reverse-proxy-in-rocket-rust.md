---
author: Fauzaan Gasim
title: How to reverse proxy framer through Rocket (Rust)
date: 2023-04-06
description: Learn how to reverse proxy your Framer application through the Rocket web framework in Rust, enabling you to serve your Framer site within a single request-response cycle.
tags: [Framer, Rocket, Rust, reverse proxy, web development]
---

## Reverse Proxying Framer with Rocket

Integrating Framer, a powerful design and prototyping tool, with a robust backend framework like Rocket (Rust) can provide a seamless user experience by reverse proxying the Framer application. This approach allows you to serve the Framer site within a single request-response cycle, improving performance and simplifying the overall architecture.

## Setting up the Rocket Server

1. **Install Rocket**: Begin by setting up a new Rust project and adding the Rocket dependency to your `Cargo.toml` file:

```toml
[dependencies]
rocket = "0.5.0-rc.2"
```

2. **Create the Rocket Server**: In your main Rust file (e.g., `main.rs`), set up the Rocket server:

```rust
#[macro_use]
extern crate rocket;

#[get("/")]
fn index() -> &'static str {
    "Hello, world!"
}

#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![index])
}
```

3. **Serve the Framer Application**: To reverse proxy the Framer application, you'll need to handle the requests for the Framer site and forward them to the Framer server. Here's an example of how you can do this:

```rust
use rocket::http::Status;
use rocket::response::Responder;
use std::io::Cursor;

#[get("/framer/<path..>")]
async fn framer(path: PathBuf) -> Result<FramerResponse, Status> {
    let framer_url = format!("https://framer.com/{}", path.to_string_lossy());
    let response = reqwest::get(framer_url).await.map_err(|_| Status::InternalServerError)?;

    if response.status().is_success() {
        let body = response.bytes().await.map_err(|_| Status::InternalServerError)?;
        Ok(FramerResponse(body))
    } else {
        Err(Status::from_code(response.status().as_u16()).unwrap())
    }
}

struct FramerResponse(Vec<u8>);

impl<'r> Responder<'r, 'static> for FramerResponse {
    fn respond_to(self, _: &'r rocket::Request<'_>) -> rocket::response::Result<'static> {
        rocket::Response::build()
            .header(rocket::http::ContentType::new("text", "html"))
            .sized_body(Cursor::new(self.0))
            .ok()
    }
}

#[launch]
fn rocket() -> _ {
    rocket::build().mount("/", routes![index, framer])
}
```

In this example, the `framer` route handles requests for the Framer application. It constructs the Framer URL based on the requested path, fetches the content from the Framer server, and then returns the response as a custom `FramerResponse` type.

The `FramerResponse` struct is responsible for responding to the client with the fetched Framer content, setting the appropriate content type and size.

## Benefits of Reverse Proxying Framer with Rocket

1. **Improved Performance**: By reverse proxying the Framer application through Rocket, you can serve the Framer site within a single request-response cycle, reducing latency and improving the overall user experience.

2. **Simplified Architecture**: Integrating Framer with a backend framework like Rocket allows you to manage the entire application within a single codebase, simplifying the overall architecture and making it easier to maintain and deploy.

3. **Seamless Integration**: Reverse proxying Framer through Rocket enables you to seamlessly integrate the design and prototyping capabilities of Framer with the robust backend features of Rocket, creating a cohesive and powerful web application.

## Conclusion

Reverse proxying Framer through the Rocket web framework in Rust is a powerful technique that can enhance the performance and simplify the architecture of your web application. By following the steps outlined in this guide, you can easily integrate Framer with Rocket, enabling you to serve your Framer site within a single request-response cycle and provide a seamless user experience.