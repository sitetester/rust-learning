# Blanket implementation

In fact, we can provide a custom method on any Rust std built-in types (with a `T: std::fmt::Debug` constraint).

Here any type T which implements `std::fmt::Debug` trait could have a custom method like the following.

```rust
use std::collections::HashMap;
trait MyPrinter {
    fn my_print(&self);
}

impl<T: std::fmt::Debug> MyPrinter for T {
    fn my_print(&self) {
        println!("MyPrinter called for: {:?}", self);
    }
}

fn main() {
    let i = 10;
    i.my_print();

    let f = 10.2;
    f.my_print();

    let my_str = "my custom &str";
    my_str.my_print();

    let s = String::from("Welcome to Rust");
    s.my_print();

    let vec = vec![1, 2, 3];
    vec.my_print();

    let map = HashMap::from([("TeamA", 30), ("TeamB", 70)]);
    map.my_print();

    #[derive(Debug)]
    struct User {
        name: String,
    }
    let u = User {
        name: "Alex".to_string(),
    };
    u.my_print();
}
```
Same for other types