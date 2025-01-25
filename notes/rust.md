---
id: 0h57aw0be2jnc3dxcpn3ajn
title: Rust
desc: ''
updated: 1737789794423
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



## Vectors

These are similar to arrays but can grow or shrink in size. 
They are stored in the heap rather than the stack which is the case for arrays.

Vectors can only store data of the same type.

The vec! macro is used to create a vector.

```rust
let v = vec![1, 2, 3];
```

## Ownership

Ownership is a set of rule governing how Rust manages memory.

Three possibilities for programming languanges:

- their is garbage collection periodically 
- the programmer must explicitily allocate and free memory
- memory is managed through a system of **ownership** with a set of rules checked by the compiler (rust)


### Stck versus heap

Think of stack as a pile of plates. _last in, first out._ You dont remove stuff from in between.
Yopu _push onto the stack_ or _pop off the stack_. All data on the stack must have known, fixed size.

On the hep, its different and less organized. The memory allocator check for a big enough spacem marks it as beeing in use and returns a _pointer._ This is called _allocating._ Because the pointer is know, fixed sized, you can in turn store it on the stack. 

Pushing on the stack is much quicker because you just have to add stuff on top. No time wasted in finding free space.
Similarly acccessing data on the stack is quicker.

#### Ownership rules

- each values in Rust as an owner.
- There can be only one owner at a time.
- When the owner goes out of scope, the value will be dropped.

#### Variable scope


The scope is the range within the programm for which an item is valid.

```rust
    {                      // s is not valid here, it’s not yet declared
        let s = "hello";   // s is valid from this point forward

        // do stuff with s
    }                      // this scope is now over, and s is no longer valid
```

What happens behind the scenes is that rust's `drop` function is automatically runned at the closing opf the curly brackets.


## References and borrowing


A reference is like a pointer in the sense that it is an adress to be followed to acess the data stored at this adress (data owned by another variable). But unlike a pointer, a reference is guaranteed to point to a valid value of a particular type for the life of that reference.


The ampersands represent a reference e.g `&s1`. Here you refer to some value, without taking ownership.

`*`is the _dereferencing_ operator.

Because we nevere had ownership when using a reference, we do not need to return the values from a function to give back ownership.
The creation of a reference is called _borrowing_.
Like in real life, if a person owns something, you can, sometimes, borrow it. When you are done you give it back, because you dont own it.


You cannot modify stuff you borrow if it is not mutable. However you can make mutable references.
One big restriction: if you have a mutable reference to a value, you cannot meke another reference to that value.


This allows rust to prevent data race condition.

This happens in the three sceanrii:

- two or more pointers access the same data at the same time.
- at least one of the pointers is beeing used to write to the data
- no mechanism used to synchronize acess to the data.

Rules of references.

- at any given time you can have either one mutable reference or any number of immutable references.
- references must always be valid.

