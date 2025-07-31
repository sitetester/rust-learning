# Rust Ownership

Ownership is Rust's most unique feature. Here's how it works:
```rust,editable
fn main() {
    let s1 = String::from("hello");
    let s2 = s1; // s1 is moved to s2

    // println!("{}", s1); // This would error!
    println!("{}", s2); // This works
}
```