# Phương thức

Các phương thức được chú thích tương tự như các hàm:

```rust,editable
struct Owner(i32);

impl Owner {
    // Chú thích về lifetime được sử dụng tương tự như đối với một hàm độc lập.
    fn add_one<'a>(&'a mut self) { self.0 += 1; }
    fn print<'a>(&'a self) {
        println!("`print`: {}", self.0);
    }
}

fn main() {
    let mut owner = Owner(18);

    owner.add_one();
    owner.print();
}
```

### Xem thêm:

[methods]

[methods]: ../../fn/methods.md