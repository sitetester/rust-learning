
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