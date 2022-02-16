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

