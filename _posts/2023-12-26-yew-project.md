---
title: Yew Project Hosted with GitHub Pages
date: 2023-12-26 
categories: [Rust,Web]
tags: [rust,yew,github-pages]
published: false
---

Recently, I have been wanting to create something with [WebAssembly (WASM)](https://webassembly.org/), so I looked up frameworks for creating frontend apps with Rust and found [Yew](https://yew.rs/). They have a nice [introduction/tutorial]() that is very easy to follow.

## Yew Introduction

First, you should install Rust, and then install [Trunk]() and add the WASM target:

```sh
rustup target add wasm32-unknown-unknown
cargo install --locked trunk
```
{: .nolineno }

Trunk makes working with WASM so much easier (there are other options linked to in the tutorial).

Now create a new cargo project, and add Yew to the dependencies:

```sh
cargo new yew_app
```
{: .nolineno }

```toml
# Cargo.toml
[package]
# ...

[dependencies]
yew = { git = "https://yewstack/yew", features = ["csr"] }
```

Now, replace the `main.rs` file contents with this:

```rs
use yew::prelude::*;

#[function_component]
fn App() -> Html {
    let counter = use_state(|| 0);
    let onclick = {
        let counter = counter.clone();
        move |_| {
            let value = *counter + 1;
            counter.set(value);
        }
    };

    html! {
        <div>
            <button {onclick}>{ "+1" }</button>
            <p>{ *counter }</p>
        </div>
    }
}

fn main() {
    yew::Renderer::<App>::new().render();
}
```

And create an `index.html` file in the root of your cargo project:

```html
<!doctype html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>Yew App</title>
    </head>
    <body></body>
</html>
```

Now you can run

```sh
trunk serve
```
{: .nolineno }

And you can see your app in your browser (optionally, you can add the `--open` flag to open it in your browser automatically).

Here are some other useful features you can configure for Trunk using a `Trunk.toml` file:

```toml
# Trunk.toml
[serve]
address = "127.0.0.1" # Address to listen on
# address = "0.0.0.0" # If you want to listen to external requests
port = 8000 # Listen on this port

dist = "build" # Folder to put the built files into (default is `dist`)
```

You can see a lot more options [here](https://github.com/trunk-rs/trunk/blob/main/Trunk.toml).

<!--

- Started basic calculator app

- Setup GitHub pages
    - Make public
    - Add source (docs)
    - Change Trunk.toml file
        - dist = "docs"
    - Add custom domain

-->
