---
title: Yew Project Hosted with GitHub Pages
date: 2023-12-26 
categories: [Rust,Web]
tags: [rust,yew,github-pages]
published: false
---

Recently, I have been wanting to create something with [WebAssembly (WASM)](https://webassembly.org/), so I looked up frameworks for creating frontend apps with Rust and found [Yew](https://yew.rs/). They have a nice [introduction/tutorial]() that is very easy to follow.

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

<!--

- Yew intro

- Started basic calculator app

- Setup GitHub pages
    - Make public
    - Add source (docs)
    - Change Trunk.toml file
        - dist = "docs"
    - Add custom domain

-->
