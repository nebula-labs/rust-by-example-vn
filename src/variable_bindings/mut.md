# Mutability (Tính khả biến)

Các ràng buộc biến là immutable (bất biến) theo mặc định; nhưng có thể ghi đè chúng
nếu sử dụng từ khóa `mut` để kích hoạt tính khả biến cho ràng buộc biến.

```rust,editable,ignore,mdbook-runnable
fn main() {
    let _immutable_binding = 1;
    let mut mutable_binding = 1;

    println!("Before mutation: {}", mutable_binding);

    // Ok
    mutable_binding += 1;

    println!("After mutation: {}", mutable_binding);

    // Lỗi!
    _immutable_binding += 1;
    // FIXME ^ Comment dòng trên lại
}
```

Trình biên dịch sẽ đưa ra một chẩn đoán chi tiết về các lỗi về tính khả biến của ràng buộc biến.

