# eframe template

[![dependency status](https://deps.rs/repo/github/emilk/eframe_template/status.svg)](https://deps.rs/repo/github/emilk/eframe_template)
[![Build Status](https://github.com/emilk/eframe_template/workflows/CI/badge.svg)](https://github.com/emilk/eframe_template/actions?workflow=CI)

This is a template repo for [eframe](https://github.com/emilk/egui/tree/master/crates/eframe), a framework for writing apps using [egui](https://github.com/emilk/egui/).



### Creating a Cargo-Generate template

Translating this repo into a Cargo-Generate template:

```zsh
echo
rm -rf dist/ target/ target_wasm/ target_ra/ fill_template.sh fill_template.ps1 flake.nix Cargo.lock
echo
sd 'emilk(\.github\.io/)eframe_template' '{{ github_username }}${1}{{ project-name }}' $(fd . -t f)
sd 'emilk(/)eframe_template'             '{{ github_username }}${1}{{ crate_name }}' $(fd --hidden . -t f)
sd 'eframe_template(_bg\.wasm|\.js)'     '{{ crate_name }}${1}'                      $(fd --hidden . -t f)
sd '(name = ")eframe_template'           '${1}{{ project-name }}'                    $(fd --hidden . -t f)
sd '([^/])eframe_template'               '${1}{{ crate_name }}'                      $(fd --hidden . -t f)
sd '([^/])eframe template'               '${1}{{ project-name | title_case }}'       $(fd --hidden . -t f)
sd '([^/])eframe.template'               '${1}{{ project-name }}'                    $(fd --hidden . -t f)
sd '(authors = \[").*("\])'              '${1}{{ authors }}${2}'                     Cargo.toml
echo
cargo generate --test
echo
rg --hidden 'emilk.*eframe_template'
rg --hidden '[^/]eframe.template'
rg --hidden 'emilk/eframe.template'
fd --unrestricted 'eframe|emilk'
```

### Learning about egui

`src/app.rs` contains a simple example app. This is just to give some inspiration - most of it can be removed if you like.

The official egui docs are at <https://docs.rs/egui>. If you prefer watching a video introduction, check out <https://www.youtube.com/watch?v=NtUkr_z7l84>. For inspiration, check out the [the egui web demo](https://emilk.github.io/egui/index.html) and follow the links in it to its source code.


### Web Locally

> [!NOTE]
> See just command `web-local` as well.

You can compile your app to [WASM](https://en.wikipedia.org/wiki/WebAssembly) and publish it as a web page.

We use [Trunk](https://trunkrs.dev/) to build for web target.
1. Install the required target with `rustup target add wasm32-unknown-unknown`.
2. Install Trunk with `cargo install --locked trunk`.
3. Run `trunk serve` to build and serve on `http://127.0.0.1:8080`. Trunk will rebuild automatically if you edit the project.
4. Open `http://127.0.0.1:8080/index.html#dev` in a browser. See the warning below.

> `assets/sw.js` script will try to cache our app, and loads the cached version when it cannot connect to server allowing your app to work offline (like PWA).
> appending `#dev` to `index.html` will skip this caching, allowing us to load the latest builds during development.

### Web Deploy
1. Just run `trunk build --release`.
2. It will generate a `dist` directory as a "static html" website
3. Upload the `dist` directory to any of the numerous free hosting websites including [GitHub Pages](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).
4. we already provide a workflow that auto-deploys our app to GitHub pages if you enable it.
> To enable Github Pages, you need to go to Repository -> Settings -> Pages -> Source -> set to `gh-pages` branch and `/` (root).
>
> If `gh-pages` is not available in `Source`, just create and push a branch called `gh-pages` and it should be available.
>
> If you renamed the `main` branch to something else (say you re-initialized the repository with `master` as the initial branch), be sure to edit the github workflows `.github/workflows/pages.yml` file to reflect the change
> ```yml
> on:
>   push:
>     branches:
>       - <branch name>
> ```

You can test the template app at <https://emilk.github.io/eframe_template/>.

## Updating egui

As of 2023, egui is in active development with frequent releases with breaking changes. [eframe_template](https://github.com/emilk/eframe_template/) will be updated in lock-step to always use the latest version of egui.

When updating `egui` and `eframe` it is recommended you do so one version at the time, and read about the changes in [the egui changelog](https://github.com/emilk/egui/blob/master/CHANGELOG.md) and [eframe changelog](https://github.com/emilk/egui/blob/master/crates/eframe/CHANGELOG.md).
