# Traits

Chú thích về thời gian tồn tại trong các phương thức trait về cơ bản cũng tương tự như trong function.
Lưu ý rằng `impl` cũng có thể có thời gian tồn tại.

```rust,editable
// Một struct với chú thích về thời gian tồn tại.
#[derive(Debug)]
struct Borrowed<'a> {
    x: &'a i32,
}

// Chú thích thời gian tồn tại cho impl.
impl<'a> Default for Borrowed<'a> {
    fn default() -> Self {
        Self {
            x: &10,
        }
    }
}

fn main() {
    let b: Borrowed = Default::default();
    println!("b is {:?}", b);
}
```

### Xem thêm:

[`trait`s][trait]


[trait]: ../../trait.md