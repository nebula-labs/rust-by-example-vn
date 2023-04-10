# Development dependencies (dependencies dùng trong quá trình dev)

Thỉnh thoảng chúng ta sẽ có nhu cầu cài đặt các dependencies chỉ để cho test (hoặc viết các ví dụ, hoặc đo lường (benchmarks)).
Những dependencies như vậy được thêm vào `Cargo.toml` ở phần `[dev-dependencies]`. Những dependencies này sẽ không được
truyền tải đến các package khác phụ thuộc vào package hiện tại.

Một ví dụ điển hình là [`pretty_assertions`](https://docs.rs/pretty_assertions/1.0.0/pretty_assertions/index.html), thứ giúp mở rộng các macro tiêu chuẩn như `assert_eq!` và `assert_ne!`
để cung cấp các phép so sánh dễ dàng quan sát với nhiều màu sắc.

File `Cargo.toml`:

```toml
# dữ liệu về crate tiêu chuẩn được bỏ qua
[dev-dependencies]
pretty_assertions = "1"
```

File `src/lib.rs`:

```rust,ignore
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;
    use pretty_assertions::assert_eq; // crate chỉ phục vụ cho mục đích test. Không thể dùng trong các đoạn code không phải test

    #[test]
    fn test_add() {
        assert_eq!(add(2, 3), 5);
    }
}
```

## Đọc thêm

[Cargo][cargo] tài liệu về việc chỉ định dependencies.

[cargo]: http://doc.crates.io/specifying-dependencies.html
