
# [thread_local!](https://doc.rust-lang.org/std/macro.thread_local.html)
_The macro wraps any number of static declarations and makes them thread local._

---
- Using `RefCell`

```rust
use std::cell::RefCell;

thread_local! {
    // static identifiers should be in upper case
    static VEC: RefCell<Vec<i32>> =   RefCell::new(vec![1, 2, 3]);
}

fn main() {
    let handle = std::thread::spawn(|| {
        // since we used `RefCell`, we need to use `with_borrow_mut`
        VEC.with_borrow_mut(|vec| {
            vec.push(123);
        });
        VEC.with_borrow(|vec| {
            println!("VEC inside spawned thread: {vec:?}");
        });
    });
    // wait for the associated thread to finish
    handle.join().unwrap();

    // let's add a new value to VEC
    VEC.with_borrow_mut(|vec| {
        vec.push(4);
    });
    VEC.with_borrow(|vec| {
        println!("VEC inside main thread: {vec:?}");
    });
}
```

This example demonstrates any changes made by one thread are not visible to other, i.e., each thread fully owns it's contents 
via [LocalKey](https://doc.rust-lang.org/std/thread/struct.LocalKey.html)

---


- Using `Cell`  
Here we use `Cell` to work with some primitive types which implement the `Copy` trait 
```rust
use std::cell::Cell;

thread_local! {
    // static identifiers should be in upper case
    // `Cell` only works with `Copy` type (types that implement `Copy` trait)
    static CONTAINER: Cell<[i32; 3]> =   Cell::new([1, 2, 3]);
    static NUM: Cell<i32> =   Cell::new(123);
}

fn main() {
    println!("CONTAINER: {:?}", CONTAINER.get());
    println!("NUM: {:?}\n", NUM.get());

    let handle = std::thread::spawn(|| {
        CONTAINER.set([2, 4, 6]);
        NUM.set(246);

        println!("CONTAINER inside spawned thread: {:?}", CONTAINER.get());
        println!("NUM inside spawned thread: {:?}\n", NUM.get());
    });
    // wait for the associated thread to finish
    handle.join().unwrap();

    CONTAINER.set([1, 3, 5]);
    NUM.set(135);

    println!("CONTAINER inside main thread: {:?}", CONTAINER.get());
    println!("NUM inside main thread: {:?}", NUM.get());
}
```

Note that we don't need to use [Arc](https://doc.rust-lang.org/std/sync/struct.Arc.html) here, since we use it in those situations when we want 
to share same data between threads, but here, our goal is to have each thread having it's own isolated data.