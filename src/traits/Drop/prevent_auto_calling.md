# Preventing Auto Drop Calling

## Using `std::mem::forget`

```rust
use std::mem;

struct User {
    name: String,
}

impl Drop for User {
    fn drop(&mut self) {
        println!("Drop called for {}", self.name);
    }
}

fn main() {
    let user = User {
        name: "Alex".to_string(),
    };
    
    // Prevent Drop from being called
    mem::forget(user);
}
```

## Using `ManuallyDrop`

```rust
use std::mem::ManuallyDrop;

struct User {
    name: String,
}

impl Drop for User {
    fn drop(&mut self) {
        println!("Drop called for {}", self.name);
    }
}

fn main() {
    let user = ManuallyDrop::new(User {
        name: "Jones".to_string(),
    });
}
```