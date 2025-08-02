# A function whose return type is either another function or a closure

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
enum FnOrClosure {
    Fn,
    Closure,
}

fn return_fn_or_closure(return_type: FnOrClosure) -> Box<dyn Fn()> {
    match return_type {
        FnOrClosure::Fn => {
            fn greeter() {
                println!("[fn] Welcome to Rust!");
            }
            Box::new(greeter)
        }
        FnOrClosure::Closure => {
            let greeter_closure = || println!("[closure] Welcome to Rust!");
            Box::new(greeter_clsoure)
        }
    }
}

fn main() {
    let returned_fn = return_fn_or_closure(FnOrClosure::Fn);
    returned_fn();

    let returned_closure = return_fn_or_closure(FnOrClosure::Closure);
    returned_closure();
}
```
Let's break this down:
- [Fn](https://doc.rust-lang.org/std/ops/trait.Fn.html) is a trait  
- [dyn](https://doc.rust-lang.org/std/keyword.dyn.html) (dynamic)is a keyword which creates trait objects (fat pointers (data + vtable)) 
- `Fn()` refers to an implementation of `Fn` trait. That means either function pointers (fn) or closures which take none params `()`. 
Alternatively, we could have `dyn Fn(i32)` or `dyn Fn(i32, i32)` with a corresponding input i32 input params
- & we used [Box](https://doc.rust-lang.org/std/boxed/index.html), because ? :)
if we just return `fn return_fn_or_closure(return_type: FnOrClosure) -> dyn Fn()`, Rust compiler will throw error 
`dyn Fn()` `doesn't have a size known at compile-time`, trait objects (`dyn Fn()`) are dynamically sized types that can't be stored directly 
on the stack, Box provides heap allocation with known/fixed size pointer.
