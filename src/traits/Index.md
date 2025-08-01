
# [Index](https://doc.rust-lang.org/std/ops/trait.Index.html)
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