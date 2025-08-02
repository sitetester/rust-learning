
# <a href="https://doc.rust-lang.org/std/ops/trait.Add.html" target="_ blank">Add</a>
```
pub trait Add<Rhs = Self> {
    type Output;

    // Required method
    const fn add(self, rhs: Rhs) -> Self::Output;
}
```
---

- Using default generic param <Rhs = Self>
```rust
use std::ops::Add;

#[derive(Debug)]
struct Point {x: f32, y: f32}

// ```pub trait Add<Rhs = Self>```
// Since the generic param defaults to `Self` (Point), hence we don't provide any param here
impl Add for Point {
    type Output = Point; // We want to return a `Point`
    
    fn add(self, rhs: Self) -> Self::Output {
        Point {
            x: self.x + rhs.x,
            y: self.y + rhs.y
        }
    }
}

fn main() {
    let p1 = Point {x: 1.2, y: 4.3};
    let p2 = Point {x: 3.4, y: 2.1};
    
    println!("p1 + p2 = {:?}", p1 + p2);
}
```
---

- `Rhs = Self` generic param explicitly provided
```rust
use std::ops::Add;

#[derive(Debug)]
struct Point {x: f32, y: f32}

impl Add<Self> for Point {
    type Output = Self; // `Self` refers to current struct  

    fn add(self, rhs: Self) -> Self::Output {
        Point {
            x: self.x + rhs.x,
            y: self.y + rhs.y
        }
    }
}

fn main() {
    let p1 = Point {x: 1.2, y: 4.3};
    let p2 = Point {x: 3.4, y: 7.2};

    println!("p1 + p2: {:?}", p1 + p2);
}
```
---

- `Rhs = Point` generic param explicitly provided
```rust
use std::ops::Add;

#[derive(Debug)]
struct Point {x: f32, y: f32}

impl Add<Point> for Point {
    type Output = Point; 

    fn add(self, rhs: Point) -> Self::Output {
        Point {
            x: self.x + rhs.x,
            y: self.y + rhs.y
        }
    }
}

fn main() {
    let p1 = Point {x: 1.2, y: 4.3};
    let p2 = Point {x: 3.4, y: 7.2};

    println!("p1 + p2: {:?}", p1 + p2);
}

```
