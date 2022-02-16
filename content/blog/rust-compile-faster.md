+++
title = "Making Rust compile faster"
date = 2022-02-16
draft = true
+++

We all know that compiling Rust code is often so slow because Rust actually prefer runtime to compile time.  However, it is possible to make Rustc compile faster. 

<!-- more -->

## But why should I care about compile time?

Although users only care about the runtime, compile time optimization bring developers some benefits:

- Cheaper CD/CI cost.
- Less time wasted when compiling after making changes to a large project.

## How to make Rust compile faster?

### Update Rust Toolchain and Compiler

Run `rustup update` to get the latest Rust version. Great efforts by Rust compiler team had made Rust compiler compile faster in its newer version.

### Split code into crates

Using *workspaces*, we can split our large crate into smaller ones to avoid repetitive compilation because only crates with changes will need recompilation.

### CI Caching

Often, our code get rebuilt from scratch on CI. We can still do incremental build by caching `~/.cargo` and `target/` from previous builds.

