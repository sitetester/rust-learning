# Custom method on built-in types

We can `impl` our custom trait for Rust std built-in types.

```rust
trait MyPrinter {
    fn my_print(&self);
}

impl MyPrinter for i32 {
    fn my_print(&self) {
        println!("MyPrinter for i32");
    }
}

fn main() {
    let i = 10;
    i.my_print();
}
```

This is just one concrete example. Same can be done for other types. 
Check - [blanket_implementation](blanket_implementation.md) for a generic implementation.
