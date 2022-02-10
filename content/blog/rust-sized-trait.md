+++
title = "Rust and its `Sized` trait"
date = 2022-02-10
+++

One of important things about Rust is `Sized` trait, which is an auto trait that get implemented automatically for for types that meet some certain criteria.  

<!-- more -->

## The `Sized` trait

`Sized` asserts that the size of the type is constant and known at the compile time. For instance, `u8` is `Sized` and its size is one byte. Meanwhile `Vec<T>` takes 12 bytes on a 32bit platform or 24 bytes on a 64bit platform. 

Pointers are also `sized`. Even pointers to unsized type are `Sized`. 

However, we can not easily tell the size of a `str` at compile time, so it is a dynamically sized type.

## `Sized` and Generic
 
A function definiton like this...

```rust
fn myfunc<T>(t: T) {}
```

is same as...

```rust
fn myfunc<T: Sized>(t: T) {}
```

When we write generic code, every generic type parameter will get bound with the `Sized` trait automatically. If we do not like that and want to allow `T` to accept unsized type, we can bound the type parameter to `?Sized`. It means "optionally sized" and will accept both unsized and sized type.

```rust
fn debug<T: Debug>(t: T) { // T: Debug + Sized
    println!("{:?}", t);
}
```