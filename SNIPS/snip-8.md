---
snip: 8
title: Cairo V1 Comment and Documentation Standard
status: Draft
type: Informational
author: Andrew Fleming <fleming-andrew@protonmail.com>
created: 2023-08-21
---

## Simple Summary

A standard for writing documentation and comments in Cairo contracts.

Inspired by Rust comment conventions as defined in [RFC-1574](https://github.com/rust-lang/rfcs/commit/9b39f573ff9fc230de7388fb515bac0794fe2e36).

## Abstract

This proposal suggests adopting Rust-like comments and documentation in Cairo smart contracts.

## Motivation

Establishing a standard for comments and documention increases code readability and enables tooling for documentation generation.
Both the Starknet contract syntax and Cairo programming language have radically transformed since the initial [SNIP-4](snip-4.md) proposal.
Because the Cairo language migrated to a Rust-inspired syntax, documentation practices should follow in this direction.

## Specification

### Markdown

Comments and documentation should be written with Markdown syntax.

### Contract documentation

Contract documentation refers to an expositional overview of the contract.
This may include a description of the contract itself and examples of use.
Contract documentation may use the characters `//!` to communicate that it is contract documentation.
This proposal recommends that contract documentation should start at the very first line of the module.

```rust
//! # MyContract and Implementation
//!
//! This is an example description of MyContract and some of its features.
//!
//! # Examples
//!
//! ```
//! #[starknet::contract]
//! mod MyContractExtended {
//!   use path::to::MyContract;
//!
//!   #[storage]
//!   struct Storage {}
//!
//!   fn foo() {
//!     MyContract.my_selector();
//!   }
//! }
```

### Contract-element documentation

Contract-element documentation refers to the specific element it's documenting e.g. contract method, event, implementation, etc.
This may include a description of the element itself, examples of use, and panicable conditions.
Some implementations may choose to include parameter and return descriptions.
Contract-element documentation may use the characters `///`.

```rust
/// Returns the sum of `arg1` and `arg2`.
/// `arg1` cannot be zero.
///
/// # Arguments
///
/// * `arg1` - A non-zero felt252 value.
/// * `arg2` - A felt252 value.
///
/// # Returns
///
/// * Felt252 `arg` value.
///
/// # Panics
///
/// This function will panic if `arg1` is `0`.
///
/// # Examples
///
/// ```rust
/// let a: felt252 = 2;
/// let b: felt252 = 3;
/// let c: felt252 = add(a, b);
/// assert(c == a + b, "Should equal a + b");
/// ```
fn add(arg1: felt252, arg2: felt252) -> felt252 {
    assert(arg1 != 0, "Cannot be zero");
    arg1 + arg2
}
```

### Line comments

Line comments refer to the specific line(s) immediately proceeding the comment.
This may include clarification, context, or extra details regarding the target line(s).
Line comments may use the characters `//`.

```rust
fn foo(){
    // Example line comment
}
```

## Implementation

Below is a full example of how Cairo documentation and comments may be used.

```rust
//! # MyContract and Implementation
//!
//! This is an example description of MyContract and some of its features.
//!
//! # Example
//!
//! ```
//! #[starknet::contract]
//! mod MyContractExtended {
//!   use path::to::MyContract;
//!
//!   #[storage]
//!   struct Storage {}
//!
//!   fn foo(a: felt252, b: felt252) -> felt252{
//!     MyContract.sum_minus_one(a, b)
//!   }
//! }
#[starknet::contract]
mod MyContract {

    #[storage]
    struct Storage {}

    /// Returns the sum  minus one of `arg1` and `arg2`.
    /// `arg1` cannot be zero.
    ///
    /// # Arguments
    ///
    /// * `arg1` - A non-zero felt252 value.
    /// * `arg2` - A felt252 value.
    ///
    /// # Returns
    ///
    /// * The sum of `arg1` and `arg2`.
    ///
    /// # Panics
    ///
    /// This function will panic if `arg1` is `0`.
    ///
    /// # Examples
    ///
    /// ```rust
    /// let a: felt252 = 2;
    /// let b: felt252 = 3;
    /// let c: felt252 = sum_minus_one(a, b);
    /// assert(c + 1 == a + b, "Should equal a + b");
    /// ```
    #[external(v0)]
    fn sum_minus_one(arg1: felt252, arg2: felt252) -> felt252 {
        assert(arg1 != 0, "Cannot be zero");
        let sum: felt252 = arg1 + arg2;
        // Subtract `1` from the sum
        sum - 1
    }
}
```

## History

## Copyright

Copyright and related rights waived via [MIT](../LICENSE).
