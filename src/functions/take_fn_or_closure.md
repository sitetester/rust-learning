# A function whose param is either another function or a closure

```
// https://doc.rust-lang.org/std/ops/trait.Fn.html
pub trait Fn<Args>: FnMut<Args>
where
    Args: Tuple,
{
    // Required method
    extern "rust-call" fn call(&self, args: Args) -> Self::Output;
}
```
---

```rust
fn take_fn_or_closure<F>(f: F)
where
    F: Fn(),
{
    f();
}

fn main() {
    // a function pointer - https://doc.rust-lang.org/std/primitive.fn.html
    fn greeter() {
        println!("[fn] Welcome to Rust!");
    }

    // A clsure which auto implements `Fn` trait
    let greeter_clsoure = || println!("[clsoure] Welcome to Rust!");

    take_fn_or_closure(greeter);
    take_fn_or_closure(greeter_clsoure);
}
```

```