# Calling same method from traits

- Simple case

```rust
trait SomeTrait1 {
    fn some_method(&self) {
        println!("SomeTrait1::some_method()!");
    }
}

trait SomeTrait2 {
    fn some_method(&self) {
        println!("SomeTrait2::some_method()!");
    }
}

struct SomeStruct;
impl SomeTrait1 for SomeStruct {}
impl SomeTrait2 for SomeStruct {}

fn main() {
    let s = SomeStruct;

    // error[E0034]: multiple applicable items in scope
    // s.some_method();

    // Option 1: Use `<T as Trait>::` Fully Qualified syntax, where T is some concrete type.
    <SomeStruct as SomeTrait1>::some_method(&s);
    <SomeStruct as SomeTrait2>::some_method(&s);
    
    println!();
    
    // Option 2: Trait method syntax
    SomeTrait1::some_method(&s);
    SomeTrait2::some_method(&s);
}
```
---
- Enhanced case

Here we add `impl SomeTrait1 for i32 {}` to enhance our example & also method signature is updated to `some_method(&self) where Self: Debug` since 
there is a debug print for `self`, indicating concrete type must implement `std::fmt::Debug` trait.

```rust
use std::fmt::Debug;

trait SomeTrait1 {
    fn some_method(&self) where Self: Debug  {
        println!("SomeTrait1::some_method() called for {:?}", self);
    }
}

trait SomeTrait2 {
    fn some_method(&self) where Self: Debug  {
        println!("SomeTrait2::some_method() called for {:?}", self);
    }
}

#[derive(Debug)]
struct SomeStruct;

impl SomeTrait1 for SomeStruct {}
impl SomeTrait2 for SomeStruct {}

impl SomeTrait1 for i32 {}
impl SomeTrait2 for i32 {}

fn main() {
    let s = SomeStruct;

    // error[E0034]: multiple applicable items in scope
    // s.some_method();

    // Option 1: Use `<T as Trait>::` Fully Qualified syntax, where T is some concrete type.
    <SomeStruct as SomeTrait1>::some_method(&s);
    <SomeStruct as SomeTrait2>::some_method(&s);
    
    println!();
    
    // Option 2: Trait method syntax
    SomeTrait1::some_method(&s);
    SomeTrait2::some_method(&s);
    
    println!();
    let i = 10;
    <i32 as SomeTrait1>::some_method(&i);
    <i32 as SomeTrait2>::some_method(&i);
}
```
`self` refers to instance, actual value, while `Self` is the relevant type, e.g., SomeStruct, i32