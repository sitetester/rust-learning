
### <a href="https://doc.rust-lang.org/std/convert/trait.From.html" target="_blank">From</a>

```rust
pub trait From<T>: Sized {
    // Required method
    fn from(value: T) -> Self;
}
```
Let's build a `Point` from `(f32, f32)` tuple 
```rust
#[derive(Debug)]
struct Point {
    x: f32,
    y: f32,
}

// `From<T>`, here T is `(f32, f32)`
impl From<(f32, f32)> for Point {
    fn from(tuple: (f32, f32)) -> Self {
        Self {
            x: tuple.0,
            y: tuple.1,
        }
    }
}

fn main() {
    let point = Point::from((1.2_f32, 3.7_f32));
    println!("{point:?}");
    
    // once we have `From` setup, we get `into()` for free
    let point: Point = (1.2_f32, 3.7_f32).into();
    println!("{point:?}");
}

```