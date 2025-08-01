# Iterator Pattern

The Iterator pattern provides a way to access elements of a collection sequentially without exposing the underlying representation. Rust has excellent built-in support for iterators.

## Basic Iterator Usage

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    
    // Using iterator methods
    let doubled: Vec<i32> = numbers
        .iter()
        .map(|x| x * 2)
        .collect();
    
    println!("{:?}", doubled); // [2, 4, 6, 8, 10]
}
```

## Custom Iterator Implementation

```rust
struct Counter {
    current: usize,
    max: usize,
}

impl Counter {
    fn new(max: usize) -> Counter {
        Counter { current: 0, max }
    }
}

impl Iterator for Counter {
    type Item = usize;
    
    fn next(&mut self) -> Option<Self::Item> {
        if self.current < self.max {
            let current = self.current;
            self.current += 1;
            Some(current)
        } else {
            None
        }
    }
}

fn main() {
    let counter = Counter::new(5);
    
    for num in counter {
        println!("{}", num); // 0, 1, 2, 3, 4
    }
}
```
