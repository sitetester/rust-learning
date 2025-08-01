# Preventing Auto Drop Calling

Can either use 
**<a href="https://doc.rust-lang.org/std/mem/fn.forget.html" target="_blank">forget</a>
or 
<a href="https://doc.rust-lang.org/std/mem/struct.ManuallyDrop.html" target="_blank">ManuallyDrop</a>**

```rust
use std::mem::forget;
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
    let user = User {
        name: "Alex".to_string(),
    };
    forget(user);

    let user = ManuallyDrop::new(User {
        name: "Jones".to_string(),
    });
}
```