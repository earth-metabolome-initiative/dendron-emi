---
id: 0h57aw0be2jnc3dxcpn3ajn
title: Rust
desc: ''
updated: 1737651116140
created: 1737625384436
---

## Functions

Rust is an expression based language.

- **Statements** are instructions that perform some action but do not return a value.

e.g. `let x = 6;`

- **Expressions** evaluate to a resulting value.

e.g. `x + 1`

## Crates

They are collection of rust code files. They are binary crates and library crates. Library crates contains code to be used in other programs an not on its own.

## Doc

If you don't remeber which traits and functions are available in your codebase you can run 

```bash
cargo doc --open
```

## Shadowing    

This is convenient when you want to reuse a variable without having to come up with a new name. 

```rust
let x = 5;
let x = x + 1;
let x = x * 2;
```



## Questions

What exactly is a variant. How is that different from a trait ?

Define 

- type
- trait
- struct
- enum
- variant
- module
- crate
- arms: an arm consist of a pattern to be matched agaisnt and the code to be run if the pattern is matched.

For example

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```
This is also the case in an if statement. Each blocks of code associated to the if and else are called arms.

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```

## Iif Expressions

In rust, if should be associated to a boolean. 

When you have more than one `else if` expression you might want to refactor your code. Rust has a powerful branching construct called `match` for these cases.

## Data types

Two types of data types in Rust:
- scalar
- compound

### Scalar

Represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters.

#### integers

Signed or unsigned. Signed can be negative or positive. Unsigned can only be positive.

Char

specified with single quote 

e.g `let c = 'z';`
unlike strings which are specified with double quotes.

### Compound

- tuples
cannot grow or shrink. fixed lenght. 
e.g. `let tup: (i32, f64, u8) = (500, 6.4, 1);`


To access values of the tuple we destructure it

```rust
let tup = (500, 6.4, 1);
let (x, y, z) = tup;

println!("The value of y is: {}", y);
```

Another way to acess elements of the tupple is by using the index

```rust
let x: (i32, f64, u8) = (500, 6.4, 1);

let five_hundred = x.0;
let six_point_four = x.1;
let one = x.2;
```

- arrays

Unlike tupples, every element must be of the same type. They are fixed lenght. 

```rust
let a = [1, 2, 3, 4, 5];
```

To acess elements of an array we use the index

```rust
let a = [1, 2, 3, 4, 5];

let first = a[0];
let second = a[1];
```

## The Slice type



