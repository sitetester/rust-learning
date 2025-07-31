### Return value from a spawned thread

```rust
fn main() {
    let handle = std::thread::spawn(|| {
        vec![1, 2, 3]
    });

    println!("{:?}", handle.join().unwrap());
}

```

### Using Scoped Threads

We can use [ScopedJoinedHandle](https://doc.rust-lang.org/std/thread/struct.ScopedJoinHandle.html)

```rust
use std::thread;

fn main() {
    let vec = thread::scope(|s| {
        let _join_handle1 = s.spawn(|| {
            println!("inside spawned thread 1");
        });

        let _join_handle2 = s.spawn(|| {
            println!("inside spawned thread 2");
        });
        // Note: We didn't need to collect these handles 
        // & then handle.join().unwrap() for each of them

        // can still return something
        vec![1, 2, 3]
    });
    
    // no need for `.join().unwrap()` inside main()
    println!("Vec: {:?}", vec);
}

```


