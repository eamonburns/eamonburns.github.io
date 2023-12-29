---
title: Yew Project Hosted with GitHub Pages
date: 2023-12-26 
categories: [Rust,Web]
tags: [rust,yew,tailwind,github-pages]
published: false
---

Recently, I have been wanting to create something with [WebAssembly (WASM)](https://webassembly.org/), so I looked up frameworks for creating frontend apps with Rust and found [Yew](https://yew.rs/). They have a nice [introduction/tutorial](https://yew.rs/docs/getting-started/introduction) that is very easy to follow.

## Yew Introduction

First, you should install Rust, and then install [Trunk](https://trunkrs.dev/) and add the WASM target:

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

> Tip: If you use Emmet abbreviations in VSCode, you can configure the the "Included languages" setting:
>
>```json
> "emmet.includeLanguages": {
>     "rust": "html"
> }
>```
>
> This way, you can have Emmet abbreviations inside of the `html!` macro!
{: .prompt-tip }

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

dist = "docs" # Folder to put the built files into (default is `dist`)
```

You can see a lot more options [here](https://github.com/trunk-rs/trunk/blob/main/Trunk.toml).

## My Calculator App

After that, I wanted to make a more useful app, so I decided to make a simple calculator.

I started by looking at [this example](https://github.com/yewstack/yew/blob/master/examples/counter/src/main.rs). It is the same basic program as the counter in the introduction, but it uses a `struct`-based component.

I won't go very in-depth, but the basic structure is this:

```rs
pub enum Msg {
    // Possible messages
}

pub struct App {
    // Fields represent the state of the app
}
impl Component for App {
    type Message = Msg;
    type Properties = ();

    fn create(/* ... */) -> Self {
        // Create component
    }

    fn update(/* ... */, msg: Self::Message) -> bool {
        // Update the state of the component based on the message
    }

    fn view(/* ... */) -> Html {
        html! {
            // Generate the html representation of the component based on its state
        }
    }
}

fn main() {
    yew::Renderer::<App>::new().render();
}
```

I made a crude proof of concept (it was kind of terrible), and decided to find out how to deploy it to GitHub pages (You can see the finished version at [yew.eamonburns.com](https://yew.eamonburns.com/)).

## Deploying to GitHub Pages

First, I had to make it public.

Then I changed the source to be the `master` branch in the `docs/` directory (The `docs/` and `/ (root)` directories are the only directories available from the settings page, that is why I changed the `dist` option in the `Trunk.toml` file).

I tried to get to it via `eamonburns.com/simple_yew_app` (the name of the repo), but that was kind of inconsistent about whether it would actually load the app properly, I decided to use GitHub Actions to deploy it instead, so that it could have a more consistent environment.

I did some more Googling and found [this GitHub workflow](https://github.com/SpanishPear/yew_worker_stylist_actions_pages_template/blob/main/.github/workflows/deploy.yml) that looked like it would do the job.

Unfortunately, it was a little outdated, so I had to update the version numbers and change a few other things (like adding a CNAME), and this is the [final workflow file](https://github.com/Agent-E11/simple_yew_app/blob/master/.github/workflows/deploy-gh-pages.yml).

(I also changed the source to the `/ (root)` directory in the `gh-pages` branch, because that is where [this GitHub Pages action](https://github.com/peaceiris/actions-gh-pages?tab=readme-ov-file#%EF%B8%8F-source-directory-publish_dir) places the files by default)

After that, on every push to the `master` branch, the site gets rebuilt and deployed to GitHub pages automagically!

## A More Functional Calculator

After I put the calculator up on GitHub pages, I wanted to make it work better, so I added a bunch of features like a backspace button, a clear button, and history.

Great! Now it _works_ but it doesn't _look nice_:

![Picture of calculator](/assets/img/yew-project_bad-calculator.png)

Now, I want to add some styles to it, and I decided to use [Tailwind.css](https://tailwindcss.com/).

<!--

TODO: Better styles with Tailwind.css

-->
