# Higher order functions

- Data type explicitly provided 
```rust
fn apply_op<F>(op: F, x: i32, y: i32) -> i32
where
    F: Fn(i32, i32) -> i32,
{
    op(x, y)
}

fn main() {
    let add = |x: i32, y: i32| x + y;
    let sub = |x: i32, y: i32| x - y;
    let mul = |x: i32, y: i32| x * y;
    let div = |x: i32, y: i32| x / y;

    println!("apply_op(add, 2, 3): {}", apply_op(add , 2, 3));
    println!("apply_op(sub, 2, 3): {}", apply_op(sub , 2, 3));
    println!("apply_op(mul, 2, 3): {}", apply_op(mul , 2, 3));
    println!("apply_op(div, 2, 3): {}", apply_op(div , 2, 3));
}
```
--- 
- Generic data type
```rust
fn apply_op<F, T>(op: F, x: T, y: T) -> T
where
    F: Fn(T, T) -> T,
{
    op(x, y)
}


fn main() {
    println!("Using i32");

    println!("apply_op(add, 2, 3): {}", apply_op(|x, y| x + y, 2, 3));
    println!("apply_op(sub, 2, 3): {}", apply_op(|x, y| x - y, 2, 3));
    println!("apply_op(mul, 2, 3): {}", apply_op(|x, y| x * y, 2, 3));
    println!("apply_op(div, 2, 3): {}\n", apply_op(|x, y| x / y, 9, 3));

    println!("Using f32");

    println!(
        "apply_op(|x, y| x + y, 2.1, 3.4): {}",
        apply_op(|x, y| x + y, 2.1, 3.4)
    );
    println!(
        "apply_op(|x, y| x - y, 2.3, 3.5): {}",
        apply_op(|x, y| x - y, 2.3, 3.5)
    );
    println!(
        "apply_op(|x, y| x * y, 7.2, 3.7): {}",
        apply_op(|x, y| x * y, 7.2, 3.7)
    );
    println!(
        "apply_op(|x, y| x / y, 20.2, 2.4): {}",
        apply_op(|x, y| x / y, 20.2, 2.4)
    );
}

```
