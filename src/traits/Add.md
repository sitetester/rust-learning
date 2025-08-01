
# [Add](https://doc.rust-lang.org/std/ops/trait.Add.html)
```rust
pub trait Add<Rhs = Self> {
    type Output;

    // Required method
    const fn add(self, rhs: Rhs) -> Self::Output;
}
```
