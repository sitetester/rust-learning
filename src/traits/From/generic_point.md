# <a href="https://doc.rust-lang.org/std/ops/trait.Add.html" target="_ blank">Add</a>
```
pub trait Add<Rhs = Self> {
    type Output;

    // Required method
    const fn add(self, rhs: Rhs) -> Self::Output;
}
```
Here we implement Add trait for a generic Point<T> 

```rust
use std::ops::Add;

#[derive(Debug)]
struct Point<T> {x: T, y: T}

// T will be i32 or f64
impl<T> Add for Point<T>
    // it must also implement Add trait with required associated type Output being set to T
    where T: std::ops::Add<Output = T> 
{
    type Output = Point<T>;
    
    fn add(self, rhs: Point<T>) -> Self::Output {
        Point {
            x: self.x + rhs.x,
            y: self.y + rhs.y
        }
    }
}

fn main() {
    // with i32 values
    let p1 = Point {x: 1, y: 4};
    let p2 = Point {x: 3, y: 7};

    println!("[i32 values] p1 + p2: {:?}", p1 + p2);

    // with f64 values
    let p1 = Point {x: 1.2, y: 4.3};
    let p2 = Point {x: 3.4, y: 7.2};

    println!("[f64 values] p1 + p2: {:?}", p1 + p2);
}


```
