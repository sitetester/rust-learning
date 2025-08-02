
# <a href="https://doc.rust-lang.org/std/ops/trait.Index.html" target="_ blank">Index</a>

```rust
pub trait Index<Idx>
where
    Idx: ?Sized,
{
    type Output: ?Sized;

    // Required method
    fn index(&self, index: Idx) -> &Self::Output;
}
```
---

```rust
use std::ops::Index;
struct MyTupleStruct(u32, u32);

// Here `<Idx> is `u32`
impl Index<u32> for MyTupleStruct {
    type Output = u32;

    fn index(&self, index: u32) -> &Self::Output {
        println!("Getting value by index: {index}");

        match index {
            0 => &self.0,
            1 => &self.1,
            _ => panic!("Valid index should be 0 or 1"),
        }
    }
}

fn main() {
    let s = MyTupleStruct(1, 3);
    
    // Rust compiler will auto convert to s.index(0) or 1 
    println!("s[0]: {}", s[0]);
    println!("s[1]: {}\n", s[1]);

    // We can make an explicit call too
    println!("s[0]: {}", s.index(0));
    println!("s[1]: {}", s.index(1));
}
```