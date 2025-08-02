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
---
- Param as array of functions (using Box)  
Since each closure has a distinct unique type in Rust, even if they have identical signatures, we use `dyn Fn(i32, i32) -> i32` syntax 
to convert them into a trait object & thus the actual method call is resolved at runtime (dynamic dispatch). Box will store each on heap 
& will us a fat pointer with a fixed / known size at compile time, which can be stored in stack. 
  
```rust
use std::collections::HashMap;

fn apply_ops<T: Copy + std::fmt::Display>(ops: HashMap<&str, Box<dyn Fn(T, T) -> T>>, x: T, y: T) {
    for (name, op) in ops {
        println!("{}({}, {}): {}", name, x, y, op(x, y));
    }
}

fn main() {
    let add = |x, y| x + y;
    let sub = |x, y| x - y;
    let mul = |x, y| x * y;
    let div = |x, y| x / y;

    let mut ops: HashMap<&str, Box<dyn Fn(i32, i32) -> i32>> = HashMap::new();
    ops.insert("add", Box::new(add));
    ops.insert("sub", Box::new(sub));
    ops.insert("mul", Box::new(mul));
    ops.insert("div", Box::new(div));

    apply_ops(ops, 2, 3);
}
```
--- 
- Param as array of functions (using function pointer)  
Here we specify closure type only once to help Rust compiler insert such closure with similar type into a HashMap. Each closure will still 
have it's own unique type, but after detecting type from first .insert(), rest of closures will be auto cast to match the type inferred 
`HashMap<K, V>` signature.   

```rust
use std::collections::HashMap;

fn apply_ops<T: Copy + std::fmt::Display>(ops: HashMap<&str, fn(T, T) -> T>, x: T, y: T) {
    for (name, op) in ops {
        println!("{}({}, {}): {}", name, x, y, op(x, y));
    }
}

fn main() {
    type SIGNATURE = fn(i32, i32) -> i32;

    let add = |x, y| x + y;
    let sub = |x, y| x - y;
    let mul = |x, y| x * y;
    let div = |x, y| x / y;

    // HashMap declared with explicit signature
    let mut ops: HashMap<&str, SIGNATURE> = HashMap::new();
    ops.insert("add", add);
    ops.insert("sub", sub);
    ops.insert("mul", mul);
    ops.insert("div", div);
    apply_ops(ops, 2, 3);

    println!("\nWith explicit annotation");
    let add: SIGNATURE = |x, y| x + y;
    let mut ops = HashMap::new(); // we didn't specify HashMap's type here
    // it has explicit SIGNATURE, after this insert, compiler will infer data type for HashMap
    // & rest of the inserts will be auto cast to match Hashmap inferred type
    ops.insert("add", add); 
    ops.insert("sub", sub); 
    ops.insert("mul", mul);
    ops.insert("div", div);
    apply_ops(ops, 2, 3);

    println!("\nWith explicit cast in definition");
    let add = (|x, y| x + y) as SIGNATURE;
    let mut ops = HashMap::new(); // we didn't specify HashMap's type here
    ops.insert("add", add);
    ops.insert("sub", sub);
    ops.insert("mul", mul);
    ops.insert("div", div);
    apply_ops(ops, 2, 3);

    println!("\nWith explicit cast during .insert()");
    let add = |x, y| x + y;
    let sub = |x, y| x - y;
    let mul = |x, y| x * y;
    let div = |x, y| x / y;
    let mut ops = HashMap::new(); // we didn't specify HashMap's type here
    ops.insert("add", add as SIGNATURE);
    ops.insert("sub", sub);
    ops.insert("mul", mul);
    ops.insert("div", div);
    apply_ops(ops, 2, 3);
}

```