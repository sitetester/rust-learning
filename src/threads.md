### Return value from a spawned thread

```rust
fn main() {
    let handle = std::thread::spawn(|| {
        vec![1, 2, 3]
    });

    println!("{:?}", handle.join().unwrap());

}

```
