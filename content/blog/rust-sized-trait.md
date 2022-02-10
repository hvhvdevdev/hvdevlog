+++
title = "Rust and its `Sized` trait"
date = 2022-02-10
draft = true
+++

One of important things about Rust is `Sized` trait, which is an auto trait that get implemented automatically for for types that meet some certain criteria.  

<!-- more -->

`Sized` asserts that the size of the type is constant and known at the compile time. For instance, `u8` is `Sized` and its size is one byte. Meanwhile `Vec<T>` takes 12 bytes on a 32bit platform or 24 bytes on a 64bit platform. 

Pointers are also `sized`. Even pointers to unsized type are `Sized`.

However, we can not easily tell the size of a `str` at compile time, so it is a dynamically sized type.


 

